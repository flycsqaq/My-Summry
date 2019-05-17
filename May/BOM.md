# DOM
Document Object Model, 对节点的增删查改及父子关系描述。常用的有创建节点及查询节点。除此之外，还可以对节点绑定事件。
## CURD
### Create
1. document.createElement(name)
2. document.createAttribute(name)
3. document.createTextNode(value)
3. el.setAttribute(attr, value)
4. innerHTML

### Retrieve
1. el.getElementById(name)
2. el.getElementsByClassName(names) // 'name1 name2' 同时包括
3. el.getElementsByTagName(name)
4. el.querySelector(selectors)
5. el.querySelectorAll(selectors)
6. el.getAttribute(name)

### parent and children
1. el.append(childEl)
2. el.parentNode
3. el.children

## 事件《红宝书13.16章》
事件是指由浏览器窗口发生的一些特定的交互瞬间，可以使用侦听器监听事件，以便在事件发生时执行某些代码。而事件大部分是由某些设备的触发的，最常见的便是鼠标与键盘了。因未开发过关于拖放、移动端及播放器的相关业务，因此未对这些有过多的了解。因为这些大概只需看文档就能开明白了吧。
### 事件触发(冒泡与捕获)
事件从DOM对象的根部向下传递(捕获)，到交互的最底层节点后再向上传递(冒泡)。但，为什么要这么设计。有个问题，浏览器是如何得知与哪个节点交互的，可能需要从根部向下搜索(捕获)，但到最底层后，为什么要再冒泡呢?可能是因为事件源本身（可能）并没有处理事件的能力。
### 鼠标(指针与节点的交互)
如果将指针看做一个质点在平面直角坐标系做无规则运动，而节点为在该坐标系中的封闭图形。那么可以以质点的运动轨迹与图形的交点为边界，描述质点在极小时间内的运动轨迹与图形的位置关系。
质点与图形的位置关系有两种，即在图形内(in)，在图形外(out);而质点的运动时间始末为t1与t2,因此质点在单位时间内的运动轨迹与图形的位置关系共有4种情况，即在外运动，进入(mouseenter)，在内运动(mousemove)，离开(mouseleave)。
而mouseover与mouseout则是mousemove的特殊情况，即图形内包含小图形，鼠标以小图形的边界为参照而触发的事件。
除此之外，鼠标还具有点击事件:mousedown, mouseup, click, dblclick
### 键盘事件
按下键盘上的按键，再放开，是完整的一次按键。这可以分解成两个步骤即按下(keydown)与放开(keyup)。
### 焦点事件
焦点，指的是
1. blur(失去焦点)
2. focus(获得焦点)
### UI事件
1. load // 资源加载完毕后触发
2. unload // 资源卸载后触发
3. error
4. resize // 浏览器调整宽度与高度后触发
5. scroll // 滚动期间触发
### 移动端(未学习)
1. 触摸
2. 手势
# BOM(Browser Object Model)
BOM对象是指宿主环境中的浏览器，大多可以通过临时查文档的方式来编程。
## window
### 窗口大小
1. outerWidth/outerHeight + innerWidth/innerHeight(包括滚动条)
2. clientWidth/clientHeight
### setTimeout/setInterval
```
const timeId = setTimeout(fn, ms)
clearTimeout(timeId)
```
## location
可以获取关于当前url的信息。
## history
保存着用户上网的历史记录。