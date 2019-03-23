本文是对github上开源项目laxxx的介绍,不到10天已有3500+star。
[传送门](https://github.com/alexfoxy/laxxx)
# 1. 简介
laxxx是一款简单轻巧（<2kb gzipped）的javascript插件,当你滚动时创建流畅和美丽的动画的开源项目，通过监听scroll,使元素在页面上活跃起来。

# 2. 初始化
将lax.js导入后需要初始化插件
```
  window.onload = function() {
    lax.setup() // init
      
    document.addEventListener('scroll', function(e) {
      lax.update(window.scrollY) // update every scroll
    }, false)
  }
```
下面从setup函数开始，逐步介绍lax.js。

## 2.1 lax对象的变量
在这里，先介绍lax对象的公共变量以及可以在外部调用的方法。

### 2.1.1 lax
1. elements: 保存了所有应用lax.js(类似于data-lax-xx)的元素节点。

2. presets: lax内置的动画效果。
3. addPreset: 为lax添加新的预设动画。

4. setup: lax初始化函数。
5. removeElement: 移除某个节点的动画效果。

6. addElement: 添加动画节点,在lax内会在初始化时自动添加。如果在初始化后，还需要再添加某个lax动画的元素节点，可以调用该方法。
7. populateElements: 该方法可以获取所有应用了lax动画的元素节点，并调用addElement将其添加到elements数组中。因此，当添加了新的多个添加了"data-lax-xx"属性时，可以调用该方法，重新获取elements数组。但该方法

8. updateElement: 该方法为单个应用了lax的元素节点更新特效(transform/filter)。在addElement/update后调用。
9. update: 该方法更新了lastY, 并为每一个elements数组中的元素节点调用updateElement。

总结：
1. elements保存了所有应用lax.js(类似于data-lax-xx)的元素节点。
2. addElement可以添加单个元素节点，populateElements可以获取DOM所有应用lax的元素节点。
3. updateElement更新单个元素节点的style(我不知道为什么要将该方法暴露在外部)，而update更新所有元素节点的style
4. lax只是一个获取元素节点，并监听scroll事件更新style的小型库。

### 2.1.2 lastY
lastY: 保存了window.scrollY的值。通过lax.update更新。

### 2.1.3 transforms
transforms: 动画键值对，key为DOM节点上的类似于"data-lax-xx"的属性，value为改变transform or filter样式的函数。例如```"data-lax-opacity": function(style, v) { style.opacity = v }```

### 2.1.4 crazy
为lax.presets.crazy提供值。

### 2.1.5 intrp
通过lastY以及"data-lax-xx"对应的值更新元素的style

## 2.2 setup
在当前版本setup只是用于调用populateElements。

### 2.2.1 populateElements
populateElements用于初始化elements,并获取所有应用lax.js的DOM元素节点,再通过addElement添加到elements。
```
  lax.populateElements = function() {
    lax.elements = []

    var selector = Object.keys(transforms).map(t => `[${t}]`).join(",")
    selector += ",[data-lax-preset]"

    document.querySelectorAll(selector).forEach(lax.addElement)
  }
```

### 2.2.2 addElement
addElement可以分解成下面几个步骤。
1. 检查该元素节点是否有预设属性"data-lax-preset"。为其转化为transfroms的key。再添加"data-lax-anchor"属性为"self"。"data-lax-anchor"属性意为转化参照物(元素节点)，未设置该属性的均以scrollY=0作为参考。
2. 性能选项: 
1) 默认情况下-webkit-backface-visibility: hidden;会添加到元素样式中，以鼓励浏览器将该对象渲染为GPU上的图层并提高性能。要关闭它，请添加data-lax-use-gpu="false"到元素中。
2) 默认情况下，不更新具有不透明度0的元素。你可以手动设置一个data-lax-opacity来自己控制或者使用data-lax-optimize，当它离开屏幕时不透明度设置为0。该属性通过getBoundingClientRect()方法获取元素高度，并设置"data-lax-opacity"。
3. 为具有""data-lax-anchor"属性的元素节点添加"data-lax-anchor-top"属性,为transforms对象添加属性，该属性的键为transform中的key,而值为[number, number]数组。类似于"0 1, 100, 0"(vw,vh,elh,elw通过正则替换成px)并转化为数组，去首尾空白，排序。```.replace().split().map().sort()```
4. 添加节点到elements,调用updateElement。
```
elements: [
  o: {
    el: el,
    transforms: {
      "data-lax-xx": [
        [number, number],
        [number, number]
      ]
    }
  }
]
```
### 2.2.3 updateElement
updateElement可以分解成下面几个步骤。
1) 检查元素是否有"data-lax-anchor-top"属性, 计算r。
例如,el.["data-lax-anchor-top"] = 100 + 0 = 100, 即该元素对于视口的左上角位置的高度为100px。"data-lax-anchor-top"的意思是当元素开始退出窗口可见区域时"data-lax-xx"第一个参数记为"0"。
例如:
```
<p data-lax-opacity="200 1, 100 1, 0 0" data-lax-anchor="self">
	I start to fade out after I'm 100px away from the top of the window
	and then I'm gone by the time I reach the top!
</p>
```
```
const y = lastY
var r = o["data-lax-anchor-top"] ? o["data-lax-anchor-top"]-y : y
```
2) 调用intrp, 更新获取新值，将该值传入对应的函数，改变style对象。
```
  var style = {
    transform: "",
    filter: ""
  }
  for (var i in o.transforms) {
    const t = transforms[i]
    t(style, intrp(o[t], t))
  }
```
3) 更新style,当style.opacity为0时，不更新
```
  for(let k in style) {
    if(style.opacity === 0) { // if opacity 0 don't update
      o.el.style.opacity = 0 
    } else {
      o.el.style[k] = style[k]
    }
  }
```

## 2.3. update
update的作用是更新lastY,并对lax的元素调用updateElement。
```
  lax.update = function(y) {
    lastY = y
    lax.elements.forEach(this.updateElement)
  }
```
# 4. 其他
## 4.1 removeElement
调用该方法移除不需要在更新的元素。
```
  lax.removeElement = function(el) {
    const i = lax.elements.findIndex(o => o.el = el)
    if(i > -1) {
      lax.elements.splice(i, 1)
    }
  }
```
# 5. 总结
## 5.1 启动
```
  window.onload = function() {
    lax.setup() // init
      
    document.addEventListener('scroll', function(e) {
      lax.update(window.scrollY) // update every scroll
    }, false)
  }
```
## 5.2 性能
1. data-lax-use-gpu="false"
2. data-lax-optimize=true 
## 5.3 data-lax-anchor
改变参照物为某元素。