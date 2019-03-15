## 1. 选择器
## 2. 变量
1. 类型
unit rgba
1. 变量
```
font-size = 14px
font = font-size "Lucida Grande", Arial

body
  font font sans-serif
```
2. 属性查找
前置@字符在属性名前来访问该属性名对应的值
```
#logo
  width: w = 150px
  height: h = 80px
  margin-left: -(w / 2)
  margin-top: -(h / 2)

#logo
  width: 150px
  height: 80px
  margin-left: -(@width / 2)
  margin-top: -(@height / 2)
```
3. 条件语句
```
position()
  position: arguments
  z-index: 1 unless @z-index 
  // 默认指定z-index值为1，但是，只有在z-index之前未指定的时候才这样

#logo
  z-index: 20
  position: absolute

#logo2
  position: absolute
```
## 3. 插值
1. 插值
与js ``` `${}` ```相似
```
vendor(prop, args)
  -webkit-{prop} args
  -moz-{prop} args
  {prop} args

border-radius()
  vendor('border-radius', arguments)

box-shadow()
  vendor('box-shadow', arguments)

button
  border-radius 1px 2px / 3px 4px
```
2. 选择器插值
```
table
  for row in 1 2 3 4 5
    tr:nth-child({row})
      height: 10px * row
```

## 4. 运算符
0. unquote去掉引号
1. 运算符优先级
2. 一元运算符
! and or - + * / % == is && || in 
3. 二元运算符
[] 通过索引获取值
js []
4. 存在操作符
```
vals = (error 'one') (error 'two')
error in vals
// => false
nums = 1 2 3
1 in nums
```
5. 混合书写
```
pad(types = padding, n = 5px)
  if padding in types
    padding n
  if margin in types
    margin n

body
  pad()

body
  pad(margin)

body
  pad(padding margin, 10px)
```
```
body {
  padding: 5px;
}
body {
  margin: 5px;
}
body {
  padding: 10px;
  margin: 10px;
}
```
6. 条件赋值
```
color := white 
color ?= white
color = color is defined ? color : white
// 三句相同
```
7. 类型检查
is a / type
```
15 is a 'unit'
// => true

#fff is a 'rgba'
// => true

15 is a 'rgba'
// => false
```
8. 颜色操作
```
#0e0 + #0e0
// => #0f0
```
9. 格式化字符串
```
'-webkit-gradient(%s, %s, %s)' % (linear (0 0) (0 100%))
// => -webkit-gradient(linear, 0 0, 0 100%)
```
## 5. 混合书写
1. 混入
js arguments
```
border-radius()
  -webkit-border-radius arguments
  -moz-border-radius arguments
  border-radius arguments
```
2. 父级引用
& 函数，混合，调用 stripe，创建条纹表格
```
stripe(even = #fff, odd = #eee)
 tr
   background-color odd
   &.even
   &:nth-child(even)
       background-color even
```
## 6. 方法
1. 函数
js 默认参数
```
add(a, b)
  a + b
body 
  padding add(10px, 5)
=>
body {
  padding: 15px;
}
```
2. 函数体
内置unit()把单位都变成px, 无视单位换算
```
add(a, b = a)
  a = unit(a, px)
  b = unit(b, px)
  a + b

add(15%, 10deg)
// => 25
```
3. 多个返回值
```
sizes = 15px 10px

sizes[0]
// => 15px
```
```
sizes()
 15px 10px

sizes()[0]
// => 15px
```
与混入相似
可显式使用(), return 关键字，返回
4. 别名，变量函数(高级函数)
5. 哈希示例
```
get(hash, key)
  return pair[1] if pair[0] == key for pair in hash

hash = (one 1) (two 2) (three 3)

get(hash, two)
// => 2

get(hash, three)
// => 3

get(hash, something)
// => null
```
6. 关键字参数
没看懂

## 7. 内置方法
1. red / green / blue / alpha
2. dark / light 
3. hue / saturation / lightness
4. push ```nums = 1 2 push(nums, 3, 4, 5)``` / unshift ```nums = 4 5 unshift(nums, 3, 2, 1)```
5. keys / values 
```
paris = (one 1) (two 2)
keys(paris)
// => one two
values(paris)
```
6. typeof / unit / match
7. abs(unit) / ceil / floor / round 
8. min / max / sum / avg
9. even / add
10. length
## 8.剩余参数
args... 会忽略, 逗号
## 9. 条件
1. if / else if / else
2. unless
## 10. 迭代
1. for in
## 11
1. @extend