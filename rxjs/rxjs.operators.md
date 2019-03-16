# 1. 功能分类
## 1.1 创建类
### 1. of(cold)
指定数据集合,被订阅时，同步调用next。
```
const source$ = Observable.of(1, 2, 3);
const source$ = of(1, 2, 3);
```

### 2. range(cold)
指定范围
```
const source$ = Observable.range(a, b);

for (let num = 0, length = b; i < length; i++) {
  next(b++)
}
```
### 3. generate(cold)
循环创建
```
const source$ = Observable.generate(
  2, // 初始值, 相当于for循环中的i=2
  value => value < 10, //继续的条件, 相当于for中的条件判断
  value => value + 2, //每次值的递增
  value => value * value // 产生的结果
);
```
### 4. repeate: (cold)
重复数据,subscribe10次，合并
```
const source$ = Observable.of(1, 2, 3);
const repeated$= source$.repeat(10);
```
### 5. empty / never / throw
立即完结 / 立即出错 / 永不完结,用于组合Observable对象
```
const source$ = Observable.empty();
const source$ = Observable.throw(new Error('Oops'));
const source$ = Observable.never();
```
### 6. intervale / timer(cold)
setInterval / setTimeout
```
const source$ = Observable.interval(1000);

let i = 0
const id = setInterval(next(i++), 1000)
```
```
const source$ = Observable.timer(1000); // 1s后吐出0

const now = new Date();
const later = new Date(now.getTime() + 1000);
const source$ = Observable.timer(later);   // 某个时间点吐出0

const source$ = Observable.timer(2000, 1000); 
//i=0 2s后每一秒吐出i++
```
### 7. from(cold)
将像Observable的对象(像数组的对象)转化为Observable
```
const source$ = Observable.from([1, 2, 3]); // 1, 2, 3
const source$ = Observable.from('abc'); // 'a', 'b', 'c'
```
### 8. fromPromise
Rxjs兼容Promise的操作符
```
const promise = Promise.resolve('good');
const source$ = Observable.from(promise);

source$.subscribe(
  console.log,
  error => console.log('catch', error),
  () => console.log('complete')
);
```
### 9. fromEvent(hot)
Rxjs连接DOM的操作符
```
let clickCount = 0;
const event$ = Rx.Observable.fromEvent(document.querySelector('#clickMe'), 'click');
event$.subscribe(
  () => {
    document.querySelector('#text').innerText = ++clickCount
  }
);
```
### 10. fromEventPattern(hot)
fromEventPattern接受两个函数参数，分别对应产生的Observable对象被订阅和退订时的动作
```
import {Observable} from 'rxjs/Observable';
import EventEmitter from 'events';
import 'rxjs/add/observable/fromEventPattern';

const emitter = new EventEmitter();

const addHandler = (handler) => {
  emitter.addListener('msg', handler);
};
const removeHandler = (handler) => {
  emitter.removeListener('msg', handler);
}
const source$ = Observable.fromEventPattern(addHandler, removeHandler);

const subscription = source$.subscribe(
  console.log,
  error => console.log('catch', error),
  () => console.log('complete')
);

emitter.emit('msg', 'hello');
emitter.emit('msg', 'world');

subscription.unsubscribe();
emitter.emit('msg', 'end');
```
### 11. ajax
### 12. repeatWhen
### 13. repeatWhen
## 1.2 转化类
## 1.3 过滤类
## 1.4 合并类
## 1.5 多播类
## 1.6 错误处理类
## 1.7 辅助工具类
## 1.8 条件分支类
## 1.9 数学和合计类

# 2. 特性操作符
## 2.1 背压控制类
## 2.2 可连接类
## 2.3 高阶Observable处理类

# 3. 静态和实例分类
## 3.1 静态
## 3.2 实例