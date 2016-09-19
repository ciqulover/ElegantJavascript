## 个人总结：如何写出优雅Javascript
### 不写分号
**不写分号能显著让代码更加优雅**

没有分号的Javascript也能正常运行？

几乎是的。不过在一些特殊的时候Javascript引擎并不会帮助你正确插入分号。

**具体的情况只有这五个符号：`+ `， `-` ，` ( `， `[ `， `/`**
没了。

也就是说，凡是新的一行代码以上述五个符号开头，那么之前一句的末尾是需要分号的。

而在实际情况中，以+，- 开头的新一行代码几乎不可能出现。

所以可能情况：
```
(function(){
  // do something
})()
```
如果之前没加分号，那么这个匿名函数外的括号会调用上一行定义的函数，匿名函数就变成了参数。记住下面的这个分号
```
;(function(){
  // do something
})()
```

还有比如这样
```
;(a == 1 || b == 1) && fn()

;[].slice.call()

;/abc/.test('abcd')
```
除了上面例子，几乎没有其他情景需要加分号了。

所以经常遇到这种情况，一个javascript文件里几百个分号全是没必要的。

**而且，通常在生产环境中会提前用uglify.js去压缩代码，或是用ES6时babel转码，这都会帮你补全分号。**

### 使用ES6
相比较ES5，ES6的语法中的结构赋值，箭头函数，模板字符串，对象的简写法等都能让代码变得干净利落。
比如一个判断奇偶的函数
```
function isEven(x){
    return x % 2 == 0
}
```
ES6：
```
const isEven = x => x % 2 == 0
```
比如字符串拼接
```
dom.innerHTML='Hello '
    + name
    + ',How you today?'
```
ES6：
```
dom.innerHTML=`Hello 
    ${name}
    How you today?`
```
### 一些优雅的写法
逻辑运算符
```
if (a == 1) {
    b()
}
//可以写成
a == 1 && b()
```
初始化变量
```
var a = obj || {}
```
三元运算符
```
var a = b % 2 == 0 ? 'even' : 'odd'
```
### 合理的命名
具体来说有这样几点：
* 方法名以动词开头，比如 `var getName = function(){}`
* 布尔值以is开头，`var isEven = function(x){return x % 2 == 0}`
* 驼峰大小写和下划线不要混用，比如`whatTheHell`和`what_the_hell`
* 下划线只有在私有对象属性时使用
* 方法内部变量尽可能短

### 变量声明

**变量声明尽可能的放在开头，这样有助于：**
* 提供一个单一地址查找到函数所有需要的局部变量
* 防止因声明提升所引发的逻辑错误
* 帮助牢记要声明的变量，尽可能地少适用全局变量

使用逗号隔开换行，而非每一行重新`var`
```
var a = 1
var b = 2
//换成
var a = 1,
    b = 2
```
有的时候也可以把逗号写在开头，这样比较方便加入新的变量
```
var a = 1
   ,b = 2
   ,c = 3
```


### 对齐方式

```
var express =   require('express')
var path =      require('path')
var favicon =   require('serve-favicon')
var logger =    require('morgan')
```
实际上用var的时候是可以用逗号的。所以在ES6中更经常出现的情况:
```
import Vue          from 'vue'
import VueResource  from 'vue-resource'
import VueValidator from 'vue-validator'
import VueRouter    from 'vue-router'
import Vuex         from 'vuex'
```
### 缩短常用方法名
```
var $ = function(x){
    return document.getElementById(x)
}
```
### 避免超长代码

如果代码长到底部的滚动条都出现了，如何算得上优雅呢。

所以要保持每一行的代码不要太长，具体有这样需要注意的地方：

* 内部不重要的过程性代码尽可能短，但至少保留语义
* 如果用到链式语法，可在 ' . '的开头换行
```
[].concat(arr).sort().forEach(fn)...
```
可以改成
```
[].concat(arr)
    .sort()
    .forEach(fn)...
```
* 字符串拼接时，用加号换行，或用ES6的模板字符串
* 如果因为逻辑运算符儿导致过长，可以将逻辑运算符换行
* 如果函数参数过长，可以将参数换行，也是没问题的
* 尽可能避免嵌套过多的`if`语句
* 如果还是很长，可以尝试2个空格缩进


### 其他细节

* 代码不同功能区之间加空行，比如每个funtion之间
* 对于数组或类数组，可以用map等函数替代for循环
* 如 `+` 号的运算符之间加空格
* 若多次引用同一外部对象的属性，则定义到方法内部


###最后

在实践中还有很多可以注意的地方，以后也会慢慢完善。
当然，也欢迎一起补充~~
写优雅的代码是很令人愉悦的。
>代码如诗~
