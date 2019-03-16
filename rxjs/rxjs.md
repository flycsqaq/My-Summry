# 1.简陋的cold Rxjs
主要参考[rxjs官方文档](https://cn.rx.js.org/manual/overview.html#h11)
## 1.1 Observable
Observable作为Rxjs的核心，变量_subscribe保存了构造该对象时传入的参数(function类型)。
其subscribe方法传入对象，以约定的方式调用_subscibe保存的函数。
```
class Observable {
  constructor(_subscribe) {
    this._subscribe = _subscribe
  }
  subscribe(observer) {
    if (typeof this._subscribe === 'function') {
      return this._subscribe(observer)
    }
  }
}
```
当然，真实的Observable并不是这么简单实现的，上述代码只是为了说明Observable只不过是一个约定罢了。

主要参考[[译] 通过构建 Observable 来学习 Observable](https://github.com/RxJS-CN/rxjs-articles-translation/blob/master/articles/Learning-Observable-By-Building-Observable.md)

## 1.2 Observer
由1.1 可知Observer大抵是一个对象，其多个属性指向函数。
```
function judgeExistFn($param) {
  if ($param && typeof $param === 'function) {
    return true
  }
  return false
}
class Observer {
  constructor(next, error, complete) {
    this.next = next
    if (judgeExistFn(error)) {
      this.error = error
    }
    if (judgeExistFn(error)) {
      this.error = complete
    }
  }
}
```
传参时，error与complete是可以省略的。

## 1.3 SafeObserver
SafeObserver,下列代码展示了next以及unsubscribe中取消订阅的功能，error与complete以及取消订阅时调用的函数的实现也大抵相似。
```
class SafeObserver {
  constructor(observer) {
    this.distination = observer
  }
  next(x) {
    if (!this.isUnsubscribed && this.distination.next) {
      this.distination.next(x)
    }
  }
  unsubscribe() {
    this.isUnsubscribed = true
  }
}
```
因此Observable中subscribe方法应扩展为
```
subscribe(observer) {
  if (typeof this._subscribe === 'function') {
    const safeObserver = new SafeObserver(observer)
    return this._subscribe(safeObserver)
  }
}
```
至此，可看出_subscibe中保存的函数可能是数据源。在Rxjs中该功能交给了各个关于传入数据的操作符Operators。

[JSBin 示例来展示 Observable 的最小化实现](https://jsbin.com/depeka/edit?js,console)

主要参考[[译] 通过构建 Observable 来学习 Observable](https://github.com/RxJS-CN/rxjs-articles-translation/blob/master/articles/Learning-Observable-By-Building-Observable.md)

