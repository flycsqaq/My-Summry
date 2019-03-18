# 1. 如果将数据流看成是一个对象，那么该对象应该有哪些方法
大抵可以模仿数组的功能，例如创建、拆分、筛选、合并、映射等。但数据流又具有时间维度，流这一概念可以追溯到牛顿的《流数术和无穷级数》。即考虑这些功能时需要考虑先后顺序。
而rxjs是由js编写的库，因此，它的功能无法超越js语言本身。
## rxjs
选择操作符主要是回答下面的问题
1. 数据的流动方式
2. 由谁来控制开关

### 维度
1. 上/下游,类似于函数的输入与输出。
2. 时间, 流的概念。 
将函数与流结合，大抵是指某个函数体通过控制时间与输入来获取输出。在这里，显然对时间的控制与上游的数据是输入。rxjs的复杂大多是在时间维度上的。为了更好的理解rxjs的各个操作符，先得对rxjs对时间的控制作出实例化的概念。
### 时间
上/下游控制时间，可以想象成上下游各有一个开关，控制着数据的流动。这个开关可以由第三方来控制。
### 流转化
上下游根据数量以及流中的数据有多种对应关系。
1. 1 --> 1 / 1 --> 多 / 多 -> 1
2. 流中的数据 1 --> 1 / 1 --> 多 / 多 -> 1
3. 数据可以转化成流。
## 1. 创建
### 1. 同步创建
1. 创建
|操作符|例子|说明|
|:--|:--|:--------|
|of|of(1,2,3)|单次同步数据流|
|range|range(0,100)|范围数据流, i++|
|generate|generate(fn1,fn2,fn3)|类似于for循环，可以通过多个操作符组合创建|
|repeate|$source.repeate(10)|重复订阅，合并|

2. 异步创建
|操作符|例子|说明|
|:--|:--|:--------|
|interval|interval(1000)|第一次吐出数据0是在1s后，然后每秒突出数据i++|
|timer|timer(1000,2000)|第一次吐出数据0是在1s后，然后每两秒吐出i++,timer(a,a)===interval(a)|
|from|from([1,2,3])|将像Observable的对象转化为Observable|
|fromPromise||rxjs兼容Promise的操作符,为什么要用到呢？|
|fromEvent|fromEvent(dom, 'click)|rxjs控制DOM的操作符|
|repeatWhen|source$.repeatWhen(notifier)|notifier吐出数据后退订并重新订阅上游|

## 2. 转化
数据被创建后，需要转化。在这里，转化是上游到下游的动作。
### 1. 映射
|操作符|例子|说明|
|:--|:--|:--------|
|map|map(x=>x*2)||
|mapTo|mapTo(1)||
|pluck|pluck('name','last')||
|bufferTime|source$.bufferTime(400)|上游开关，每400s关闭一个开关，并将数据保存在数组内|
|bufferCount|bufferCount(5，6)|上游开关，每5个数据关闭，间隔6个数据重新开启，即错过第六个数据|
|bufferWhen|bufferWhen(notifier)|notifier吐出数据，关闭开关|
|bufferToggle|bufferToggle($open,$close)|$open开启，$close关闭|

### 2. 过滤
|操作符|例子|说明|
|:--|:--|:--------|
|filter|||
|first|||
|last|||
|take|||
|takeLast|||
|takeWhile|takeWhile(fn)||
|takeUntil|takeUntil(notifier$)|notifier$吐出数据，上游开关关闭|
|skip||跳过n个拿之后的数据|
|skipWhile|||
|skipUntil||notifier$吐出数据，上游开关开启|
|throttleTime|throttleTime(2000)|通过一个数据后，开关开启，记录2000s内通过的所有数据，吐出第一个数据，然后退订，等下一个数据通过时，开启开关|
|debounceTime|debounceTime(2000)|通过一个数据后，开始计时，若2000s内无数据再通过则吐出，否则，重新订阅|
|throttle和debounce|throttle(durationSelector)|由durationSelector吐出数据，控制开关|
|auditTime和audit||与throttle相似，只是把最后一个数据传给下游|
|sampleTime和sample||与audit相似，只是由下游控制开关。因此，时间是均匀的|
|distinct|distinct(null, Observable.interval(500))|只返回没出现过的数据。500ms后，因为接收到interval的数据，因此重新订阅|
|distinctUntilChanged|distinctUntilChanged(compare)|与上一个数据比较，false =>通过|

## 3. 合并
|操作符|例子|说明|
|:--|:--|:--------|
|concat|||
|merge|||
|zip|||
|combineLast|||
|widthLatestFrom|||
|race|||
|startWidth|||
|forkJoin|||

## 4. 辅助工具类
|操作符|例子|说明|
|:--|:--|:--------|
|count|||
|min / max|||
|reduce|||
|every|||
|find|||
|find / findIndex|||