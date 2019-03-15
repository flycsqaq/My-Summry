# 1. 什么是CSS
通过选择元素来设置样式
## 如何工作
浏览器 -> 载入HTML -> 输出HTML -> 创建DOM节点 -> 载入 CSS -> 输出 CSS -> CSS渲染DOM节点 -> 输出
CSS 通过改变DOM节点的某些属性的属性值来工作
## CSS语句
1. @import 元数据
2. @media 媒体查询
## 选择器
## 伪类
## 伪元素

# 2. 选择器
## 2.1 简单选择器
1. 元素
2. class
2. id
## 2.2 属性选择器
1. [attr]
2. [attr=val]
3. [attr~=val]
4. [attr|=val]
5. [attr^=val]
6. [attr=$val]
7. [attr*=val]
## 2.3 伪类
1. :hover / active / visited / focus 
2. :first-child / last-child 
3. :first-of-type / last-of-type
4. :nth-child() / :nth-of-type()
5. :required / valid / disabled
## 2.4 伪元素
1. ::after / before
2. ::first-letter / first-line
3. ::selection / backdrop
## 2.5 组合器
1. A, B
2. A B
3. A > B
4. A + B
5. A ~ B
## css层叠
1. 重要性 !important
2. 选择器 内联 / id / 类 / 元素
3. 顺序 
# 3. 布局
1. 先渲染子元素，再渲染父元素
2. inline, float元素依附于父元素，可互相感知
3. float可感知到block，而block无法感知float
4. inline-block按照inline的感知能力找到自己的位置，随后再将自己渲染成盒子

## 3.1 盒模型
### 渲染
1. 盒子的渲染是递归的过程，先渲染子盒子，再渲染父盒子
2. 渲染顺序：渲染分为盒子以及内容；inline / inline-block 先渲染内容，再渲染盒子，渲染盒子的过程中，根据有无block规定api是否有意义。block: 先渲染盒子，再渲染内容
3. inline能知道float，
4. block反馈，
5. overflow,盒模型无法知晓自身，因此需要先渲染文本,再将文本高度反馈给盒子
### api
1. margin + border + padding + width/height; 
2. box-sizing: content-box / border-box
3. overflow: auto / hidden / visible / scroll
4. outline
5. display: block / inline / inline-block
inline:依附于父元素
### 背景裁剪
background-clip: 默认 border-box

## 3.2 正常布局
1. display: block; 盒子直接换行，且占据整行
2. inline-block; 文档连接inline，再创建盒子，如果盒子创建失败，则换行
3. inline; 文档直接相连接，内容若超出盒子，超出部分直接换行
4. float: none; 无浮动，文档流中；
5. position: static; 正常文档流中；

## 3.3 float
left, right, none
1. 盒子的渲染是递归的过程，先渲染子盒子，再渲染父盒子
2. 渲染顺序：渲染分为盒子以及内容；inline / inline-block 先渲染内容，再渲染盒子，渲染盒子的过程中，根据有无block规定api是否有意义。block: 先渲染盒子，再渲染内容
3. inline能知道float，
4. block反馈，
5. overflow,根据盒模型的width/height属性渲染文本，再渲染盒子
## 3.4 position
### 3.4.1 static
### 3.4.2 relative
改变元素自身的位置，但不改变其在其他元素对其的感知
### 3.4.3 absolute
脱离文档流，寻找第一个position!==static的父元素
当该元素渲染完毕后在渲染
left / right / top / bottom 寻找上下文
再渲染盒子
### 3.4.4 fixed
根据html元素定位
### 3.4.5 sticky
## 3.5 flex
### flex-flow
1. flex-direction
2. flex-wrap
### flex
1. flex-grow
2. flex-shrink,
3. flex-basis
### 对齐（5）
4. justify-content
5. align-items
### 子元素
6. order

# 4. 样式
## 4.1 文本
### 4.1.1 字体
1. font-size
2. font-weight
3. font-family
4. font-style
5. text-decoration
6. text-shadow
7. text-align
8. line-height
9. letter-spacing
10. word-spacing
11.
```
 @font-size {
  font-family: '';
  src: url();
}
```
### 4.1.2 列表
1. list-style-type
2. list-style-image
3. list-style-position
### 4.1.3 背景
background
color
image
position
repeat
attachment
size
clip
### 4.1.4 transform
1. translate
2. scale
3. rotate
4. skew
5. scale3d
6. matrix
# 5. 动画
## 5.1 transition(过渡)
名称 + 效果时间 + 速度曲线 + 延时
## 5.2 animation(动画)
动画名称 + 效果时间 + 速度曲线 + 延时 + 播放次数 + 是否反向播放
## 5.3 @feyframes(关键帧)
