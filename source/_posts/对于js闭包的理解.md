---
layout: posts
title: JavaScript 的闭包
date: 2018-09-15 22:30:51
tags: Javascript
---

### 什么是闭包（Closure）

> **简单讲，闭包就是指有权访问另一个函数作用域中的变量的函数。**

> **MDN 上面这么说：闭包是一种特殊的对象。**它由两部分构成：函数，以及创建该函数的环境。环境由闭包创建时在作用域中的任何局部变量组成。

### 作用域

要理解闭包，就得理解Javascript函数作用域。
作用域无非就两种：全局作用域和局部作用域
看下面一个例子

```javascript
    var a = 2 // 全局作用域
    function foo(){
        var a = 3 // 局部作用域
        console.log(a) // 使用局部作用域中的变量 a，则输出2
    }
    foo()
    console.log(a) // 使用全局作用域中变量 a，则输出3
```

根据作用域的规则，引擎会从当前执行的作用域开始查找变量，如果找到，则使用该变量，如果找不到，就向上一级继续查找，直到抵达最外层的全局作用域时，无论找到
还是没找到，查找的过程都会停止。

```javascript
    var a = 2
    function foo(){
        console.log(a) //使用全局作用域中的变量 a,所以输出的是2
    }
```

```javascript
    function foo(){
        var a = 2
    }
    console.log(a) //无法访问foo()作用域中定义的量变a,全局作用域又未定义变量，因此输出的是ReferenceError: a is not defined
```


变量 a 在全局作用域下定义，则全局作用域下的局部作用域内的执行代码或者说是表达式都可以访问到变量 a 的值。局部变量里的同名变量（a）会截断对全局变量 a 的访问，因此在调用函数 foo()时会输出2。**JavaScript作用域限制了普通方法是无法让外层作用域访问到内部作用域**，因此在全局中输出变量a会是3。

```javascript
    var a = 2 // 全局作用域
    function foo1(){
        var a = 3 // 局部作用域
        function foo2(){
            console.log(a) //使用的是 foo 的局部变量，所以输出的是3
        }
        return foo2
    }

    var result = foo1()

    result() //输出的是3
```

### 闭包是如何产生的？

 实际上，上一节代码中的 foo1函数，就是闭包。
 简单来讲，**`当函数外部能访问函数内部时`**，产生的就是闭包。我理解的是，闭包就是能在全局作用域调用局部作用域，所以闭包就是将函数外部和函数内部连接起来的一座桥梁。

### 闭包的用途

闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。
在正常情况下，我们在外部时无法修改函数内部变量的值：

```javascript
    function print(x) {
    var _internal = 1;

    console.log(_internal + 1);
    }

    print(1); // 2
    // ...
    print(1); // 2
```

我们可以看的，无论print()调用多少次，输出的值都是2，_internal的值是1

这是因为 JavaScript 中的垃圾回收机制，在多次调用 print() 时，每一次都需要回收前一次的内存，之后再次申请新内存，因此 _internal 无法在内存中继续保存。

那我们如何保存值，那就是使用---**闭包**

```javascript
    var add 
    function print() {
        var _internal = 1;

        add = function(x) {
            _internal += x;
        }

        return function log() {
            console.log(_internal);
        }
    }

    var test = print();
    test(); // 1
    add(1);
    test(); // 2
```

经过上述可以看出，函数 print() 在经过 add() 运行之后，_internal 的值分别为 1 和 2，这就说明了 _internal 始终保存在内存中，并没有在 var test = print(); 调用时被回收。

这是因为 print() 内的 log() 作为返回值，被赋给 test 这个全局变量，因此 log() 始终在内存中。而 log() 依赖 print() 并且可以访问 _internal，所以 print() 也始终在内存中，而且在 var test = print(); 调用时没有被回收。

换而言之，当 _internal 在声明的时候分配了内存，我们可以将其内存地址表示为 0x...1，在 print() 函数被调用之后应该会被回收，但是由于上述原因，没有被回收，它的值将继续保留在地址为 0x...1 中。在外部可以使用指针去寻址，并取得其值。


### 闭包的弊端

- 内存泄漏：由于闭包会使得函数内部的变量都被保存在内存中，不会被销毁，内存消耗很大。因此需要在退出函数之前，将不使用的变量都删除。

- 会修改函数内部变量的值。

### 参考链接

[学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
[理解闭包](https://segmentfault.com/a/1190000009795532)
[讲清楚之javascript作用域](https://segmentfault.com/a/1190000014980841)