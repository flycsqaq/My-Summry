# CSS揭秘
## 1. 原则
## 2. 背景与边框
### 2.1 半透明边框
[demo](https://jsbin.com/didiwig/3/edit?html,css,output)
1. hsla
2. background-clip: 背景裁剪

### 2.2 多重边框
[demo](https://jsbin.com/yejacax/2/edit?html,css,output)
1. box-shadow: x y blur(模糊距离) r color
2. outline: r type color

### 2.3 灵活的背景定位
[demo](https://jsbin.com/yehucoz/7/edit?html,css,output)
1. background-position
2. background-origin
3. background-clip

## 2.4 边框内圆角
[demo](https://jsbin.com/qorepop/2/edit?html,css,output)
1. border-radius
2. box-shadow(以border-radius后的边框为基础)
3. outline(以未border-radius的边框为基础)
box-shadow控制半圆角
outline控制外边框

## 2.5 背景条纹
[demo](https://jsbin.com/fileyiy/3/edit?html,css,output)
1. 线性渐变 linear-gradient => 水平、垂直条纹
2. repeating-linear-gradient => 斜条纹

## 2.6 复杂的背景图案
[demo](https://jsbin.com/quwaxuq/7/edit?html,css,output)<br>
网格 gridding / 波点 wave / 棋盘 checkerboard

