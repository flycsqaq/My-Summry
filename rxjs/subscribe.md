# 从subscribe出发窥探Observable与Observer
## 1. 起点(函数柯里化)
### 1.1 柯里化
[jsbin在线调试](https://jsbin.com/jivodadoga/1/edit?js,console)
```
function origin() {
  return add(...arguments)
}
function add() {
  const nums = [...arguments]
  return nums.reduce((a, b) => a + b)
}

console.log(origin(1))
console.log(origin(1,2))
```
这个demo中省略了对传入参数的检验以及错误处理。函数柯里化本质上是指调用某个函数对其他参数进行解析。既然如此，可以将这些函数挂载在origin对象上。
### 1.2 Origin对象
[具有add方法的Origin对象](https://jsbin.com/quzelorexu/2/edit?js,console)
```
function Origin(data) {
  this.data = data
}
Origin.prototype.add = function() {
  return this.data.reduce((a, b) => a + b)
}

const demo = new Origin([1,2,3])
const sum = demo.add()
console.log(sum)
```
除此之外，还可以将函数当成参数保存在Origin对象中，构造多种具有不同计算方法的Origin对象。
### 1.3 进阶的Origin对象
[jsbin进阶的Origin对象](https://jsbin.com/zaquqaxuko/1/edit?js,console)
```
function Origin(fn){
  this.fn = fn
}

Origin.prototype.calculate = function(nums) {
  return this.fn(nums)
}

function add(nums) {
  return nums.reduce((a, b) => a+ b)
}

const demo = new Origin(add)
const result = demo.calculate([1,2,3])
console.log(result)
```
如你所见，这是一个不稳定的Origin对象，需要遵守强烈的契约才能运行，且没有任何的参数检验以及错误处理。这个Origin对象与Observable对象的运行方式相反，Cold Observable是传入产生数据的函数，再传入计算方法，而Origin对象则是先传入计算方法，再传入数据。但根据该对象，构建一个运行方式相反的对象应该是毫无难度的。

### 1.4 由函数产生数据的Origin
[jsbin函数产生数据demo](https://jsbin.com/zaquqaxuko/4/edit?js,console)
```
class Origin {
  constructor(datasource) {
    this._datasource  = datasource
  }
  calculate(fn) {
    const data = new SafeData(this._datasource)
    return fn(data)
  }
}

class SafeData {
  constructor(datasource) {
    this.store = []
    datasource(this)
    return this.store
  }
  save(num) {
    this.store.push(num)
  }
}

const demo = new Origin(function(dataStore) {
  dataStore.save(1)
  dataStore.save(2)
  dataStore.save(2)
})
function add(data) {
  return data.reduce((a, b) => a + b)
}
const result = demo.calculate(add)
console.log(result)
```
这是一种极其不稳固的契约，意在说明一种能联动两个函数的粗糙方法。

## 2.subscribe
### 2.1 初窥subscribe
下面是一个subscribe的调用方式。
```
const source$ = Observable.of(1, 2, 3)
$source.subscribe(
  (x) => console.log(x), 
  err => console.log(err), 
  () => console.log('done'))
```
如你所见，跟1.3的Origin相似，但这里的of操作符是创建Observable对象，并传入产生数据的函数。
```
const observer = () => {
  next(1)
  next(2)
  next(3)
}
const source$ = new Observable(observer)
```

### 2.2 婴儿期的Observable与Observer
[rough Observable](https://jsbin.com/jifulezoto/3/edit?js,console)
因篇幅所限，下面仅展示next方法。都缺少参数检验以及错误处理。
```
class Observable {
  constructor(_subscribe) {
    this._subscribe = _subscribe
  }
  subscribe(observer) {
    const myObserver = new SafeObserver(observer)
    return this._subscribe(myObserver)
  }
}

class SafeObserver {
  constructor(observer) {
    this.distination = observer
  }
  next(x) {
    if (this.distination.next && !this.isClose) {
      this.distination.next(x)
    }
  }
}
```
### 2.3 异步的Observable与Observer