# 1. JavaScript
JavaScript是一门单线程语言，即线性的，但有定时器接口，可以让某些任务延时进行。垃圾清理是载体的实现，并非语言本身。也有其独特的变量搜寻机制(作用域)。

# 2. 语言本身
## 2.1 作用域
活动对象(变量对象),向上搜索变量。
## 2.2. 闭包
即在当前作用域调用其他作用域内的变量。
## 2.3. 原型链
每个对象都有其构造函数，原型就是构造函数的prototype指向的对象。this关键字依照作用域搜索变量。
## 2.4. 垃圾清理
垃圾清理是载体的任务，大部分是标记清理，即n为指向该对象的变量数，当n为0时，该对象在下一次清理时被清理。

# 3. BOM
## 3.1 Window对象
### 1. 宽 / 高
1. innerWidth / innerHeight
### 2. 窗口移动
1. moveTo
2. moveBy

# 4. DOM
## 4.1 基本DOM操作(document)
### 1. 获取元素
1. querySelector('a') => get first element 
2. querySelectorAll('a') => gel all elements
3. getElementById() / getElementsByTagName / getElementsByClassName
### 2. 创建 + 放置 元素
1. createElement() / createTextNode
2. Node.appendChild(node)
### 3. 移动和删除元素
1. Node.appendChild(node) // 移动节点
2. removeChild() // 删除节点
3. cloneNode() // 复制节点
4. linkPara.parentNode.removeChild(linkPara) // 删除自身
### 4. 操作样式
1. para.style
2. pare.setAttribute('class', 'highlight) // 设置属性

## 4.2 html5
### 1. 焦点管理
focus()
### 2. 页面滚动
scrollIntoView()

## 4.3 事件
### 1. 事件流
1. 捕获
2. 冒泡
this指该元素，因为是现在该元素作用域调用的
### 2. 事件类型
1. ui: select / resize / scroll
2. 焦点: blur / focus
3. 鼠标+滚轮: click + dbclick + mousedown + mouseup / mouseenter + mouseleave / mousemove + mouseout + mouseover
4. 文本
5. 键盘
6. 合成
7. 变动
8. 变动名称
### 3. 事件委托

# 5. 疑问
## 5.1 为什么访问prototype中的属性需要创建一个实例