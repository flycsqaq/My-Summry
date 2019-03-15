# CSS揭秘
## 1. 原则
#58a #fb3 yellowgreen #655
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

### 2.4 边框内圆角
[demo](https://jsbin.com/qorepop/2/edit?html,css,output)
1. border-radius
2. box-shadow(以border-radius后的边框为基础)
3. outline(以未border-radius的边框为基础)
box-shadow控制半圆角
outline控制外边框

### 2.5 背景条纹
[demo](https://jsbin.com/fileyiy/3/edit?html,css,output)
1. 线性渐变 linear-gradient => 水平、垂直条纹
2. repeating-linear-gradient => 斜条纹

### 2.6 复杂的背景图案
[demo](https://jsbin.com/quwaxuq/7/edit?html,css,output)<br>
网格 gridding / 波点 wave / 棋盘 checkerboard

### 2.7 伪随机背景
[demo](https://jsbin.com/meheweh/2/edit?html,css,output)
多色垂直条纹，最小公倍数

### 2.8 连续的图像边框
[demo](https://jsbin.com/xeqinam/2/edit?html,css,output)
1. bankground-clip
2. background-position
双层背景

### 2.9 总结
1. transparent: 透明色
2. background-clip: 背景裁剪, 背景图片裁剪的框(border, padding, content)
3. background-position: 背景图片放置的初始位置
4. background-origin: 定义position的框
5. linear-gradient: 角度 开始(默认为后一种颜色) 过渡色 过渡色 结束(默认为前一种颜色)
6. repeating-linear-gradient 背景图片内重复
7. radial-gradient:
8. box-shadow: 以border-radius后的border为基扩张
9. outline: 以border-radius前的border为基扩张, 可能与引入该属性的时间有关
10. 多背景拼接成复杂图形

## 3. 形状
### 3.1 自适应椭圆
[demo](https://jsbin.com/ledudid/2/edit?html,css,output)
border-radius: x1 - x4 / y1 - y4
x  椭圆的水平半轴， y 垂直半轴
如果两个相邻的角的半径和超过了对应的盒子的边的长度，那么浏览器要重新计算保证它们不会重合。
相邻两个角的radius超过邻接边长度时，会按照两角radius加起来等于邻接边的长度来压缩所有角的radius的值。

### 3.2 平行四边形
[demo](https://jsbin.com/sisodoc/2/edit?html,css,output)
1. transform: skewX
2. ::before
3. absolute 偏移量设置为零，以便让它在水平和垂直方向上都被拉伸至宿主元素的尺寸

### 3.3 菱形
[demo](https://jsbin.com/tabuyum/1/edit?html,css,output)
clip-path: polygon(x1 y1, x2 y2 ……)
裁剪坐标，左上为0 0

### 3.4 切角效果
[demo](https://jsbin.com/zelahin/1/edit?html,css,output)
1. 切角 linear-gradient 顺时针旋转的角度
2. 弧形切角radial-gradient(circle at bottom right, white 15px, #58a 0)
3. 裁剪路径

### 3.5 梯形
无法理解，需要补充3D转化知识
[demo](https://jsbin.com/gevozof/2/edit?html,css,output)
transform运用
scaleY: 图形在Y轴长度缩放转换
perspective: 为3D转换元素定义透视视图
rotateX: 定义沿着X轴的3D旋转
transform-origin 转化源位置

### 3.6 饼图
[demo](https://jsbin.com/valuqec/3/edit?html,css,output)
css 利用图形覆盖制作
1. border-radius
2. rotate
3. animation
4. step-start / step-end 将动画分成多段，起始 / 结束 突变， 中间过程无变化
5. 动画暂停 delay负值
svg
1. svg定义外部框
2. circle fill 填充色 stroke 画

### 3.7 总结
1. border-radius x1-x4 /y1-y4
2. ::before absolute transform
3. clip-path polygon(x1 y1, x2 y2 ……)
4. animation停止动画 animation-delay animation-play-state paused
5. svg circle fill stroke

## 4. 视觉效果
### 4.1 单侧投影
[demo](https://jsbin.com/docigik/2/edit?html,css,output)
1. box-shadow 第四个参数，缩放投影尺寸

### 4.2 不规则投影
1. filter drap(x y z z(s) color)

### 4.3 染色
滤镜组合
filter: sepia(1) saturate(4) hue-rotate(295deg)

### 4.4 毛玻璃
### 4.5 折角
[demo](https://jsbin.com/fupokal/2/edit?html,css,output)
1. background linear-gradient no-repeat 位置 / 大小 
### 4.6 总结
1. box-shadow
2. filter

## 5. 字体排印
### 5.1 连字符断行(web端chorme不支持)
[demo](https://jsbin.com/zufociv/2/edit?html,css,output)
设置lang
```hyphens: auto```

### 5.2 插入换行
[demo](https://jsbin.com/vopaluz/2/edit?html,css,output)
display inline-block: white-space: pre无效
盒子会忽略换行符
```
dd::after {
  content: '\A'
}
```

### 5.3 文本行的斑马条纹
[demo](https://jsbin.com/lowuxaz/1/edit?html,css,output)
1. line-height
2. background-size
3. background-origin

### 5.4 tab宽度
现在似乎tab缩进没有这个问题了？
```pre tab-size:2```
### 5.5 连字
[demo](https://jsbin.com/rahawon/2/edit?html,css,output)
```font-variant-ligatures```
### 5.5 华丽的&符号
unicode-range: U+26

### 5.6 自定义下划线
[demo](https://jsbin.com/gehuxif/2/edit?html,css,output)
渐变

### 5.7 现实中的文字效果
[demo](https://jsbin.com/gajehe/2/edit?html,css,output)
1. 凸版印刷：hsl text-shadow
2. 空心字效果: 多个text-shadow / svg
3. 文字外发光效果:  多个text-shadow
4. 文字凸起: text-shadow
### 5.8 环形文字
svg

### 5.9 总结
1. hyphens: auto
2. after, content: '\A', white-pre: pre
3. tab-size
4. text-shadow

## 6 用户体验
### 6.1 鼠标光标
1. not-allowed
2. none
### 6.2 矿大可点击区域
[demo](https://jsbin.com/xemaxod/2/edit?html,css,output)
1. box-shadow inset
2. ::after
### 6.3 自定义复选框
