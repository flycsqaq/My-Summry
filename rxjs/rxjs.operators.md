# 1. 功能分类
## 1.1 创建类
### 1. of(cold)
指定数据集合,被订阅时，同步emit。
```
const source$ = Observable.of(1, 2, 3);
const source$ = of(1, 2, 3);
```

### 2. range(cold)
指定范围
```
const source$ = Observable.range(a, b);

for (let num = 0, length = b; i < length; i++) {
  emit(b++)
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
const id = setInterval(emit(i++), 1000)
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
```
const notifier = () => {
  return Observable.interval(1000);
};

const source$ = Observable.of(1, 2, 3);
const repeated$ = source$.repeatWhen(notifier);
// 每隔一秒，重复订阅
```
```
const notifier = (notification$) => {
  return notification$.delay(2000);
};

const repeated$ = source$.repeatWhen(notifier);
// 每当repeatWhen上游完结的时候，这个notificaton$就会吐出一个数据。
```
### 13. defer
```
const observableFactory = () => Observable.of(1, 2, 3);
const source$ = Observable.defer(observableFactory);
// 被订阅时才创建Observable
```
## 1.2 转化类
### 1. map
```
@param1 {fn} 
@param2 {string} context fn的this指向
const source$ = Observable.of(3, 1, 4);
const mapFunc = function(value, index) {
  return `${value} ${this.separator} ${index}`;
};
const context = {separator: ':'};
const result$ = source$.map(mapFunc, context);
result$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
```
```
const source$ = Observable.of(3, 1, 4);
const context = {separator: ':'};
const mapFunc = (function (separator) {
  return function(value, index) {
    return `${value} ${separator} ${index}`;
  };
})(context.separator)
const result$ = source$.map(mapFunc);
```
### 2. mapTo
映射为同一数据
```
const result$ = source$.mapTo('A');
```
### 3. pluck
指定多个参数，map(x => x.xx),优点: 自动处理字段不存在的情况
```
const source$ = Observable.of(
  {name: 'RxJS', version: 'v4'},
  {name: 'React', version: 'v15'},
  {name: 'React', version: 'v16'},
  {name: 'RxJS', version: 'v5'}
);
const result$ = source$.pluck('name');
result$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
```
### 4. windowTime和bufferTime
1. windowTime: 高阶Observable
```
@param1 {number} 持续时间
@param2 {number} 开始的时间间隔
@param2 {number} 数据个数
const source$ = Observable.timer(0, 100);
const result$ = source$.windowTime(400);
// const result$ = source$.windowTime(400, 200);
// 0,1,2,3 => 2,3,4,5
```
2. bufferTime: 产生数组的Observable
```
const result$ = source$.bufferTime(400);
```
### 5. windowCount和bufferCount
1. windowCount: 高阶Observable
```
@param1 {number} 计数的个数
@param2 {number} 计数的区间
```
2. bufferCount
### 6. windowWhen和bufferWhen
```
const source$ = Observable.timer(0, 100);
const closingSelector = () => {
  return Observable.timer(400);
};
const result$ = source$.windowWhen(closingSelector);
// closingSelector产生数据时，退订并重新订阅上游
const closingSelector = () => {
  return Observable.fromEvent(document.querySelector("#click", "click"));
};
```
### 7. windowToggle和bufferToggle
```
const source$ = Observable.timer(0, 100);
const openings$ = Observable.timer(0, 400);
const closingSelector = value => {
  // value为openings$传递的数据
  return value % 2 === 0 ? Observable.timer(200) : Observable.timer(100);
};
const result$ = source$.windowToggle(openings$, closingSelector);
// 控制开启和关闭，
```
### 7. window和buffer
控制开关
```
const source$ = Observable.timer(0, 100);
const notifer$ = Observable.timer(400, 400);
const result$ = source$.window(notifer$);
```
### 8. 高阶map
参数为返回Observable的函数，可以看成是处理异步的一种方式，$source开启一个Observable，然后高阶map操作符将Observable抚平，返回Observable。
```
const project = (value, index) => {
    return Observable.interval(100).take(5);
}
```
1. concatMap
拖拽
// source$将产生的数据传入project, project返回Observable产生数据，然后由concatMaP合并，数据指project产生的数据
// 而source$可以认为是开启一个新的Observable
```
const source$ = Observable.interval(200);
const result$ = source$.concatMap(project);
```
2. mergeMap
处理Ajax请求
```
const sendButton = document.querySelector('#send');
Rx.Observable.fromEvent(sendButton, 'click').mergeMap(() => {
  return Rx.Observable.ajax(apiUrl);
}).subscribe(result => {
   // 正常处理AJAX返回的结果
});
```
3. switchMap
新产生Observable，退订之前的Observable
4. exhaustMap
先产生的Observable的优先级总是最高
### 9. 高阶MapTo
参数为Observable对象
1. concat / merge / switch 
### 10. expand
所有expand传递给下游的数据，同时也会传递给自己，就像是逐层“展开”所有的数据。
### 11. 数据分组
将一个Observable拆分成多个Observable
1. groupBy
```
const intervalStream$ = Observable.interval(1000);
const groupByStream$ = intervalStream$.groupBy(
   x => x % 2
);
```
2. partition
```
const source$ = Observable.timer(0, 100);
const [even$, odd$] = source$.partition(x => x % 2 === 0);

even$.subscribe(value => console.log('even:', value));
odd$.subscribe(value => console.log('odd:', value));
```
### 12. 累计数据
1. scan
维持状态
```
const source$ = Observable.interval(100);
const result$ = source$.scan((accumulation, value) => {
  return accumulation + value;
});
```
## 1.3 过滤类
实例类
### 1. filter
### 2. first
```
@params1 {fn} 判别式
@params2 {fn} 结果选择器，例如 (value, index) => [value, index]
@params3 {any} 默认值, 若上游完结但无满足判别式的值
```
### 3. last
@params 与first 相同
### 4. take
1. take: 从上游拿n个数据 ```result$ = source$.take(n)```
2. takeLast: 上游完结时，取最后的n个数据
3. takeWhile: 
```
@params {fn} 判别式
let i = 0
while(boo) {
  i++
}
take(i)
``` 
4. takeUntil 
控制上游开关的关闭
```
const source$ = Observable.interval(1000);
const notifier$ = Observable.timer(2500);
const takeUntil$ = source$.takeUntil(notifier$); // 2.5S后关闭开关
```
```
const source$ = Observable.interval(1000);
const notifier$ = Observable.throw('custom error');
const takeUntil$ = source$.takeUntil(notifier$);  // 出错后关闭开关
```
### 5. skip
1.skop: 跳过N个之后全拿 ```const skip$ = source$.skip(3)```
2. skipWhile: 跳过第一个满足条件之前的所有数据 
```
@params {fn} 判别式
const source$ = Observable.interval(1000);
const skipWhile$ = source$.skipWhile(value => value % 2 === 0);
```
3. skipUntil
控制上游开关的开启
### 6. throttleTime和debounceTime
上游控制开关
1. throttleTime: 在限制时间内，只有一个数据通过
```
const source$ = Observable.interval(1000);
const result$ = source$.throttleTime(2000);
```
2. debounceTime: 在限制时间内，只有一个数据产生，则通过.否则，重新订阅上游。
```
const source$ = Observable.interval(1000);
const result$ = source$.debounceTime(2000);
// 无数据通过
```
### 7. throttle和debounce
```
const source$ = Observable.interval(1000);
const durationSelector = value => {
   return Observable.timer(value % 3 === 0 ? 2000 : 1000);
};
const result$ = source$.throttle(durationSelector);
// 0, 2, 3, 5, 6
```
```
const source$ = Observable.interval(1000);
const durationSelector = value => {
   return Observable.timer(value % 3 === 0 ? 2000 : 1000);
};
const result$ = source$.debounce(durationSelector);
// 1, 2, 4, 5
```
### 8. auditTime和audit
与throttle相似，只是把最后一个数据传给下游。若为满足条件上游就完结，则传递完结信号。
### 9. sampleTime和sample
与audit相似，只是由下游控制开关
```
const notifer$ = Rx.Observable.fromEvent(document.querySelector('#sample'), 'click');
const tick$ = Rx.Observable.timer(0, 10).map(x => x*10);
const sample$ = tick$.sample(notifer$);
sample$.subscribe(value => {
  document.querySelector('#text').innerText = value
});
```
### 10. distinct
不同，只返回没出现过的数据。
```
@params {fn} 指定
@params {Observable} 产生数据就清空数据集
const source$ = Observable.of(
  {name: 'RxJS', version: 'v4'},
  {name: 'React', version: 'v15'},
  {name: 'React', version: 'v16'},
  {name: 'RxJS', version: 'v5'}
);
const distinct$ = source$.distinct(x => x.name);

const source$ = Observable.interval(100).map(x => x % 1000);
const distinct$ = source$.distinct(null, Observable.interval(500));
```
### 11. distinctUntilChanged
与上一个数据比较
```
@params {fn} 比较
const source$ = Observable.of(
  {name: 'RxJS', version: 'v4'},
  {name: 'React', version: 'v15'},
  {name: 'React', version: 'v16'},
  {name: 'RxJS', version: 'v5'}
);
const compare = (a, b) => a.name === b.name;
const distinct$ = source$.distinctUntilChanged(compare);
```
### 12. distinctUntilKeyChanged
11的简化写法
···
const distinct$ = source$.distinctUntilKeyChanged('name');
···
### 12.其他
1. ignoreElement: 忽略数据，只关心error与complete
2. elementAt: 把上游数据当数组，只获取指定下标的那一个数据
```
@param2 {any} 默认数据
const source$ = Observable.of(3, 1, 2);
const result$ = source$.elementAt(3, null);
```
3. single 检查上游是否只有一个满足条件的数据 true => data; false => error
## 1.4 合并类
### 1. concat
首尾相连,必须等前一个Observable完结，后一个才能登场。
```
const source1$ = Observable.of(1,2,3)
const source2$ = Observable.of(4,5,6)
const concated$ = $source1.concat(source2$)
const concated2$ = Observable.concat(source1$, source2$)
```
### 2. merge
先到先得，即处理异步时，只要Observable上游emit就立即转给下游Observable,可选参数concurrent,指定同时合并的对象个数，只有前面对象完结，后面的对象才能登场。
```
const source1$ = Observable.timer(0, 1000).map(x => x+'A');
const source2$ = Observable.timer(500, 1000).map(x => x+'B');
const merged$= Observable.merge(source1$, source2$);

merged$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
```
```
0A
0B
1A
1B
2A
2B
```
### 3.zip
拉链式组合，一对一合并，无论同/异步。由于数据产生速度不同，可能会产生数据挤压问题。
吐出数据最少的上游Observable决定了zip产生的数据个数。<br>
其中一个上游完结就会完结
```
1. 一对一合并
const source1$ = Observable.of(1, 2, 3);
const source2$ = Observable.of('a', 'b', 'c');
const zipped$ = Observable.zip(source1$, source2$);

zipped$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
// 输出
[ 1, 'a' ]
[ 2, 'b' ]
[ 3, 'c' ]
complete
```
### 4. combineLatest
合并当前的最后一个数据，可选参数project,为函数，默认为 () => [...arguments]。
1. 实例操作符
source$1,source2$其中一个产生数据，就产生数据数组。result$,当results$保存了上游对象最后一个数据，数据更新且数据不为空时时，产生数据数组。<br>
所有上游完结才会完结。
```
const source1$ = Observable.timer(500, 1000);
const source2$ = Observable.timer(1000, 1000);
const result$ = source1$.combineLatest(source2$);

result$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
```
2. 多重依赖问题，即上游数据来源互相依赖产生的问题。
因为，同步只是指执行完上一步，马上执行下一步，但下例中，source1$执行完，后一步是result$
```
const original$ = Observable.timer(0, 1000);
const source1$ = original$.map(x => x+'a');
const source2$ = original$.map(x => x+'b');
const result$ = source1$.combineLatest(source2$);

result$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
```
### 5. widthLatestFrom
只有实例操作符, 合并最后一组数据，source1$控制节奏，source2$只负责提供数据。
解决了gitch问题？？不是未知问题，这是可预见的。
```
const source1$ = Observable.timer(0, 2000).map(x => 100 * x);
const source2$ = Observable.timer(500, 1000);
const result$ = source1$.withLatestFrom(source2$, (a,b)=> a+b);

result$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
// output
101
203
```
### 6. race
最先产生数据的控制节奏。
### 7. startWidth
只有实例操作符。先调用emit产生数据，再控制上一个Observable产生数据
```
const original$ = Observable.timer(0, 1000);
const result$ = original$.startWith('start');

result$.subscribe(
  console.log,
  null,
  () => console.log('complete')
);
```
### 8. forkJoin
只有静态。合并所有上游所产生的complete前的最后一个数据 ()=> [...arguments]
## 1.5 多播类
## 1.6 错误处理类
## 1.7 辅助工具类
### 1. 数学类
都是实力操作符，必须上游完结后才给下游传递唯一数据
1. count 统计数据个数
2. max / min 最大/最小值，接受参数(fn,类似于sort(fn))
3. reduce 与数组的reduce相似，第二个参数为初始值
### 2. 条件布尔类
1. every
2. find // 吐出value
3. findIndex // 吐出key
4. isEmpty  
5. defaultIfEmpty // 判断enpty吐出默认值
## 1.8 条件分支类
## 1.9 数学和合计类

# 2. 特性操作符
## 2.1 背压控制类
## 2.2 可连接类
## 2.3 高阶Observable处理类
### 1. concatAll
当数据源与需要subscribe的对象只有一个，但需要运用唯一数据源创造多个数据时运用高阶Observable，创建多个内部Observable。
```
const ho$ = Observable.interval(1000)
  .take(2)
  .map(x => Observable.interval(1500).map(y => x+':'+y).take(2));
const concated$ = ho$.concatAll();
// output
0:0 -> 0:1 -> 1:0 -> 1:1
```
### 2. mergeAll
### 3. zipAll
### 4. combineAll
### 5. switch
切换到最新的Observable对象获取数据。即由上游的Observable控制节奏
```
1:0 -> 1:1 -> complete
```
### 6. exhaust
耗尽，前一个内部Observable还没有完结，而新的Observable又已经产生。忽略新的Observable
```
const ho$ = Observable.interval(1000)
  .take(3)
  .map(x => Observable.interval(700).map(y => x+':'+y).take(2));
const result$ = ho$.exhaust();
//output
0:0 -> 0:1 -> 2:0 -> 2:1
```
# 3. 静态和实例分类
## 3.1 静态
## 3.2 实例