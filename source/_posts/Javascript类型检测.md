---
title:  Javascript类型检测
date: 2019-05-01 21:00:00
tags: javascript
---

# Javascript类型检测

## 对象与基本类型

JS中类型有7种：

* Number
* String
* Boolean
* Null
* Undefined
* Symbol
* Object

其中除了Object外都是基本(原始)类型，这意味着它们的值是不可变的，因为它们仅仅是值，并没有属性。Number、String、Boolean被函数/方法调用时，JS会将其包装成Object(基本包装类型)。Null和undefind同样是基本(原始)类型,但是没有对应的方法。

Object为引用类型，Object类型的对象有：

* Object
  
  * Function
  * Array
  * Date
  * RegExp
  
  


## typeof

 **`typeof`** 操作符返回一个字符串，表示未经计算的操作数的类型。

```js
// 基本类型
typeof 1; // number
typeof ''; // string
typeof true; // boolean
typeof undefined; // undefined

// 对象(引用)类型
typeof []; // object
typeof {}; // object
typeof new Date() // object
typeof /test/i; // object
// 以下两种简单包装对象没有用处。避免使用它们。
typeof(new String("abc")) // object 
typeof(new Number(1)) // object

// 例外
typeof null; // object
typeof function () {}; // function
```

1. 对于简单类型(null除外)，typeof返回表示对应类型名字的字符串，如`number` `string`等。

2. 对于任何Object引用类型(function除外)，typeof返回字符串`object`。

3. 例外：

```js
typeof null; // object
typeof function () {}; // function
```



## Object.prototype.toString.call()

返回值一个表示该对象的字符串："[object *type*]"， 可以用来检测**对象**类型。

即使是基本类型也可以返回正确的类型字符串。

```js
Object.prototype.toString.call([]); // [object Array]
Object.prototype.toString.call({}); // [object Object]
Object.prototype.toString.call(''); // [object String] // 基本(简单)类型
Object.prototype.toString.call(new Date()); // [object Date]
Object.prototype.toString.call(1); // [object Number]  // 基本(简单)类型
Object.prototype.toString.call(function () {}); // [object Function]
Object.prototype.toString.call(/test/i); // [object RegExp]
Object.prototype.toString.call(true); // [object Boolean]
Object.prototype.toString.call(null); // [object Null] // 基本(简单)类型
Object.prototype.toString.call(); // [object Undefined] // 基本(简单)类型
```

