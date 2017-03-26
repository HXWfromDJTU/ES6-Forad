#let 和 const 指令

##let指令

* let仅在区块代码中有效
* let的使用不存在变量提升(var的使用存在变量提升)
* let不能够未声明先使用
* 在let的“先使用”到“再声明”这种不正确的做法情况下，这个区间称之为“暂时性死区”（temporal dead zone，简称 TDZ）
* 在let声明之前，使用``typeof``对该变量进行操作仍会出现``ReferenceError``
* let在同一个作用域内，不允许重复声明，(注意：var与let也不能够重复)<pre>
<code>// 报错
function () {
  let a = 10;
  var a = 1;
}
// 报错
function () {
  let a = 10;
  let a = 1;
}
</code></pre>

##块级作用域
* 块级作用域中，外层无法读取内层的变量
* 在ES6中，允许块级作用域中声明函数，但和``let``关键字类似的是，该函数在块级作用域之外也不可引用。
* 块级作用域的出现，令“立即执行函数表达式(IIFE)”闭包的使用减少。

##const指令
* 对于const来说，只声明不赋值，就会报错。
* const声明的常量，也与let一样不可重复声明。
* const常量声明后，会指向一个对象（JS对象、数字对象、字符对象等），这个对象本身不允许改变，但允许给这个对象添加/修改属性。可使用``const foo = Object.freeze({});``创建一个不可修改的freeze，被冻结了的对象。

##ES 6 中的变量声明
* var
* function
* let
* const
* import
* class
### 全局对象和属性
* var命令和 function命令声明的全局变量，在ES 6中仍然是全局的属性
* let命令、 const命令、class命令声明的全局变量，不属于全局对象(window或global)的属性
* 全局变量逐步与全局对象的属性脱钩

