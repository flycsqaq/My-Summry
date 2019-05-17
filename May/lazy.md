# 懒加载
懒加载并不是一项新颖、困难的技术，恰恰相反，是一个简单的尝试。从资源的获取方式来看，与ajax请求获取数据并无区别。都是触发某个条件，委托网络请求资源。因此，懒加载的关键在于URI与DOM的对应关系以及请求触发的时机。
## URI与DOM
最简单也最直观的应该是key、value的对应。但除此之外还需要某些参数来判断是否发送请求。因此，其数据可以用```{ el, uri, args }```表示。
1. el保存着dom节点;
2. uri保存资源的位置;
3. args保存的参数用于判断是否发送请求。

那么，对于懒加载来说，需要的步骤也就显而易见了
1. 获取dom节点
2. 添加加载逻辑
3. 增加某些监听事件
4. 触发监听事件
5. 根据判断条件决定是否加载
## 1. 获取dom节点
获取dom节点的方式有很多,如果是写插件类的代码，只需要从中选择一个方法并记录在文档中以便查阅。当然，还可以将获取节点的代码交给开发者来做。例如：
```
class Lazy {
  constructor({ parent = document, search = 'lazy' }) {
    this.parent = parent
    this.search = search
    this.elements = []
  }
  popularElements() {
    const elements = this.parent.querySelectorAll(this.search)
    if (elements) {
      this.elements = Array.from(elements).map(el => ({ el }))
    } else {
      console.error('No element found')
    }
    return this
  }
}
```
```
class Lazyy {
  constructor({ elements }) {
    this.elements = elements ? Array.from(elements).map(el => { el }) : []
  }
}
```
## 2. 加载图片
加载图片只需将节点的src属性指向uri。因此重点如何判断是否加载图片。
```
function loadImage(o) {
  if (judge(o)) {
    o.el.src = uri
    // doSomething
  }
}
```
### getBoundingClientRect
getBoundingClientRect可以用于获取元素以视觉视口为参照物的相对坐标。可以在启动懒加载时将相对坐标转化为绝对坐标。
```
function changeAbsoluteDirection(o) {
  const { left, right, top, bottom } = o.el.getBoundingClientRect()
  const { scrollX, scrollY } = window
  return {
    left: left + scrollX,
    right: right + scrollX,
    top: top + scrollY,
    bottom: bottom + scrollY
  }
}
function judge(o) {
  const { left, right, top, bottom } = o.coordinates
  const { lastY, lastX, width, height } = this.district // 触发监听事件更新scrollX,scrollY, innerWidth, innerHeight
  const booLeft = left <= lastX
  const booRight = right >= lastX + width
  const booTop = top <= lastY
  const booBottom = bottom >= lastY + height
  return booLeft && booRight && booTop && booBottom
}
```
### intersectionObserver
```
var io = new IntersectionObserver(callback, options)
io.observe(elements)
```
## 3. 侦听器
与添加侦听器对应的，更为重要的便是移除侦听器。因此需要以一种方式保存回调函数。
```el.addEventListener(event, this.fn)```
当所有图片加载完毕，或者手动停止加载时，移除监听器。
```
if (something) {
  el.removeEventListenr(event, fn)
}
```

## 4. 触发监听事件
需要处理this的指向问题。
```
this.fn = this.update.bind(this)
function update() {
  const district = this
  // 更新scrollX,scrollY, innerWidth, innerHeight
  if (judgeEvent()) {
    this.elements.forEach(loadImage, this)
  }
}
```
judgeEvent是一个函数可以是时间判断,也可以是距离判断;该函数判断取决于你的业务逻辑，因此该函数应该由开发者控制。例如：
```
@params this.judge: { fn, name, lastValue, num }[]
function judgeEvent() {
  const iterator = this.judge.values()
  let result = false
  for (var j of iterator) {
    if (something) {
      result = true
    }
  }
  return result
}
```
## 5. 总结
这种业务可以抽象成一种简单的逻辑，即数据变化 -> 触发回调函数。可以使用proxy进行自动更新。