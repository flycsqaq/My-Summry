# 内存管理、作用域及闭包
## 内存管理
JavaScript创建变量时分配内存，并且在不再使用它们时"自动"释放。后一个过程称为垃圾回收。而闭包则是基于垃圾回收的技术。
## 垃圾回收
准确判断内存是否需要释放是一件困难的事情。
### 引用计数法
一个简单的做法便是对引用的对象进行计数，退出该作用域时将计数为0的对象回收。在循环引用时会有限制。
### 标记-清除算法
还有一种方式便是从全局作用域出发，搜索引用的对象，将未引用的对象清理。这种做法会清除所有无法从跟队形查询到的对象。<br>
从2012年起，所有现代浏览器都使用了标记-清除垃圾回收算法。
## 作用域
从根部出发，当进入某个执行环境(函数、块级作用域)时，会创建新的执行环境及变量对象。退出环境时，如果该环境被某个变量引用，那么环境中的变量对象将难以得到释放。可能引起内存泄漏。例如：
```
function test() {
  const a = 1
  return function() {
    return a
  }
}
const b = test()
```
变量b引用了匿名函数，导致该匿名函数的内存无法释放，而该匿名函数应用了test的函数作用域，因此test内的变量a无法得到释放。将该技术称之为闭包。
## 闭包
引用红宝书中关于闭包的例子。
```
function createFunctions() {
  var result = new Array()
  for (var i = 0; i < 10; i++) {
    result[i] = function() {
      return i
    }
  }
  return result
}
const demo = createFunctions()
demo[0]() // 10
demo[1]() // 10
```
该例子使用了闭包，但主要原因在于for循环
>使用var声明的变量不是该循环的局部变量，而是与for循环处在同样的作用域中。

因此该for循环等同于下例：
```
function createFunctions() {
  var result = new Array()
  var i = 0
  for ( ; i < 10; i++) {
    result[i] = function() {
      return i
    }
  }
  return result
}
```
如果使用let则会生成局部变量。