# 概况
1. 基本类型: Number, String, Boolean, undefined, null
2. 引用类型：Object
对于数据来说，基本操作是比较与交换。增加、删除和改动是交换的结果。由此可以引申出数据能否增加、删除、改动的api以及如何增删改的api。
3. 在对基本类型进行方法调用时，会先隐式地将其转化为引用类型，再通过原型链调用方法(获得属性)。

# Number
对于Number类型的数据，与现实世界中的数字相对应，整数、小数、有穷、进制、是否有意义。
## 判断api
首先进行进行类型判断，再进行值判断
```
Number.isFinite(x) // Number.isFinite(NaN) = true
Number.isInteger(x) // Number.isInteger(1.0) = true
Number.isNaN(x)
Number.isSafeInteger(x) // [-(2^53 - 1), 2^53 - 1 ]
```
## 转化api
### Number.parseFloat/parseInt
1. 将其第一个参数转换为字符串，解析它，并返回一个整数或NaN。第一个参数的类型可以是Sting/Number/Object,有趣的是```Number.parseFloat([0, 1]) // 0```。
2. parseInt可以传递第二个参数(默认为10),解析为第一个参数实际的进制，例如 parseInt(11, 2)表示为二进制下的11转化为十进制整数，结果自然是3。
### Number.prototype.methods: Number -> String
toExponential/ toFixed/ toLocaleString/ toPrecision/ toString<br>
常用：
1. toFixed(n): 一个数值的字符串表现形式, n为小数点后n位。 ```(4.55).toFixed(1) // 4.6```
2. toString(m): m为m进制。```(5).toString(2) // 101```
## why prototype
需要调用对象的当前值。

# String
对于String类型的数据，与现实世界中的语言相对应，需要做到截取、拼接、搜索，以及不同语言之间的转化。但对不同编码还未曾了解。且字符串api过多，下面只列出常用api且均在prototype上，其余见mdn文档。
## 截取
1. charAt(index)
2. slice(begin, end)
3. subString(begin, end)
4. trim()
5. trimLeft()
6. trimRight()

## 拼接
1. concat(str1, str2)
2. padEnd(targetLength, padString) // 用padString填充字符串末尾，直到达到targetLength长度
3. padStart(targetLength, padString) // 用padString填充字符串开头，直到达到targetLength长度
4. repeat(count) // 重复count次
5. replace(regex, substr) // regex匹配的内容被substr替代

## 搜索
1. endsWithin(str, position = string.length) // position位置是否是以str结尾
2. startsWithin(str, position = 0) // position位置是否是以str开头
3. includes(str, position = 0) // 从position开始搜索，是否包含str
4. indexOf(str, fromIndex) // 从fromIndex开始向后搜索，返回第一次出现str的位置
5. lastIndexOf(str, fromIndex) // 从fromIndex向前开始搜索，返回第一次出现str的位置

## 正则搜索(未学习)
1. match(regex)
2. matchAll(regex)
3. search(regex)

## 转化
1. str -> arr: split(str, limit) // limit 数组长度
### 大小写
1. toLowerCase
2. toUpperCase
3. toLocalLowerCase
4. toLocalUpperCase

# Object
## prototype/__proto__/
1. Object.prototype.__proto__ = null 为原型链的终点。
2. __proto__链查询
## 创建
1. Object.create(prototype, obj)
2. Object.assign(target, source) // source自身的可枚举属性复制到target
## 数据描述符
1. configurable // writable + delete
2. enumerable
3. value
4. writable
5. get
6. set 
### api Object.methods
1. defineProperties(obj, {})
2. defineProperty(obj, prop, {}) // 
3. getOwnPropertyDescriptors(obj)
4. getOwnPropertyDescriptor(obj, prop) // { data api }
5. freeze(obj) // 冻结对象，无法增删改
6. preventExtensibel(obj) // 阻止扩展，无法增加
7. seel(obj) // 密封，无法增删
## 获取属性 Object.methods
1. getOwnPropertyNames(obj) // obj自身拥有的枚举或不可枚举属性名称字符串 => names[]
2. entries(obj) // 给定对象自身可枚举属性的键值对数组 =>[key, value][]
3. fromEntries(obj) // [key, value][] => obj
4. for ... in // 给定对象及原型链中可枚举属性
5. getPrototypeOf(obj) // => __proto__
6. keys(obj) // 包含对象自身的所有可枚举属性key的数组
6. values(obj) // 包含对象自身的所有可枚举属性value的数组
## judge Object.methods
1. isFrozen(obj) 
2. isExtensible(obj) // 无法增加属性
3. isSealed() // 无法增删
4. is(v1, v2) // 

## prototype
1. hasOwnproperty(prop) // 
2. isPrototypeOf(obj) // 对象是否在原型链上
3. propertyIsEnumerable(prop) // 是否可枚举

# Function
想象一下，如果没有Function, 对象的继承可能是这样子的。下面以prototypeOrigin为起点
```
const b = {} // b.__proto__隐式地指向prototypeOrigin
const c = {}
b.__proto__ = c // 修改原型链
```
但这样做需要引入类来引入批量生产对象的能力。
而js则是用构造函数来模拟类。因此需要引入原始的构造函数来生成Function。通过Function生成构造函数有以下规则
1. 生成prototype属性,且prototype.constructor指向该函数。除Object之外,prototype.__proto__指向Object.prototype
2. __proto__指向Function.prototype
3. Function.prototype.__proto__ === Obejct.prototype
而通过构造函数生成对象
```
function a() {}
const b = new a()
b.__proto__ === Function.prototype // true
```
如果要深入Object与Function的话。
Object.prototype 与 Function.prototype是最原始的。而Object是由Function生成的构造函数。因此```Object.__proto__ === Function.prototype```。很巧妙的设计，但有点绕。
因此，通过Object比通过Function生成对象少一层原型链。但Function可以批量生产对象。
## api
1. bind(this, arg1, arg2)
2. call(this, arg1, arg2)
3. apply(this, [arg1, arg2])

## 函数内部关键字
### arguments
参数列表，类数组。```Array.from(arguments)``` 转化为数组。
### this
调用函数的环境，紧接的上一次get操作的结果。

# Array
数组是Function构造的构造函数，继承Object生成的对象。
```
Array.__proto__ === Function.prototype
Array.prototype.__proto__ === Object.portotype
```
1. 数组是通过有序kye存储value的结构。其键为字符串。
2. 对于数组来说，需要增删查改的功能。
## CURD
### Create
1. unshift
2. push
### Update
1. arr[n] = x // 这是在对象中通过key修改value的方法
2. copyWithin(target, start, end) // [1, 2, 3, 4].copyWithin(1, 2, 3) => [1, 3, 3, 4]
3. fill(value, start, end)
### Retrieve
1. find(func) // =>value
2. findIndex(func) // => index
3. includes(value, fromIndex)
4. indexOf(value, fromIndex) // => index
5. lastIndexOf(value, fromIndex)
### Delete
1. shift
2. pop
3. splice(position, deleteCount, addItem1, addItem2)
## 转化
1. Array.from(likeArray) // => Array
2. enties() // => Iterator [key, value]
3. keys() // => Iterator key
4. join(str) // => String
5. map(func)
6. reduce(func)
7. reduceRight(func)
## 判断
1. Array.isArray(item) // => Boolean
2. every(func)
3. some(func)
## 创建
1. Array.of(item1 item2)
## 连接
1. concat
## 筛选
1. filter(func)
## 压平
1. flat(depth)
2. flatMap(func) // Array.map(func).flat(1)
## 排序
1. sort(func) // func
2. reverse()
## 遍历
1. forEach(func, thisArg)