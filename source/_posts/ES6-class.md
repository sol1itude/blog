---
title: ES6原生Class知识
date: 2018-12-21 16:26:50
tags:
---
# 前言
js的原生Class直到ES6才实现，而Class已经在其他语言中实现了几十年，现在我们研究一下原生Class。

本文大量参考阮一峰老师的ES6手册。

# 浏览器支持情况


IE方面，由于国内需要支持到IE8，所以支持Class就别想了。
移动方面，包括微信浏览器、QQ浏览器、其他主流浏览器，都支持Class，所以移动端可以放心用。

# ES5之前定义构造函数的常见方法

```javascript
// 先定义一个函数，强行叫它构造函数，大写的P也不是必须的，只是约定俗成
function Point(x, y) {
  this.x = x; // 构造函数的属性都定义在函数内部
  this.y = y; // this指向实例对象
}

// 构造函数的方法都定义在构造函数的原型上
Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

// new 一个对象，就OK了
var p = new Point(1, 2);
```

# ES6定义类的常见方法

由于要兼容过去的构造函数写法，所以ES6的类其实就是语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用ES6的class改写，就是：

```javascript
//定义类
class Point {
  constructor(x, y) { // 定义构造方法
    this.x = x; // this指向实例对象
    this.y = y;
  }

  toString() { // 定义一个方法，注意这里没有function关键字
    return '(' + this.x + ', ' + this.y + ')'; // this指向实例对象
  }
}

```

# class就是function的另一种写法，本质还是function

```javascript
class Point {
}

typeof Point // "function" 类的数据类型就是函数
Point === Point.prototype.constructor // true 类本身就指向构造函数
```

# 类里面能定义什么，不能定义什么？

由于class只是构造函数的语法糖，所以class内部能定义什么有很多限制，截止2018年上半年，规定如下：
能定义：

构造方法
若干个实例方法
静态方法（就是类本身的方法）

有限定条件的定义：
实例属性：不能在构造方法外部定义实例属性，只能在构造方法里定义实例属性。
绝对不能定义：
不能定义静态属性（就是类本身的属性），只能在class外部定义
所以，实例属性必须在`constructor(){}`里面；静态属性必须在class的外面。
这些名词的具体解释见下文。

# 如何使用类

必须用new，不用new会报错。
```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return this.x + this.y;
  }
}

var a = new Point(10, 20);
console.log(a.toString()); // 30

```

# class也有prototype

class的所有方法（比如上面的toString()方法），都定义在prototype上面，这跟构造函数其实是一致的，只不过构造函数是显式的写出来，class是隐式定义。

在类的实例上面调用方法，其实还是调用原型上的方法。跟构造函数情况一样。

```javascript
class B {}
let b = new B();

b.constructor === B.prototype.constructor // true
```

# 给类增加新动态方法

`Object.assign`方法可以很方便地一次向类添加多个动态方法。

由于类的动态方法都定义在prototype对象上面，所以类的新动态方法添加在prototype对象上面就可以了。

```javascript
class Point {
  constructor(){
    // ...
  }
}

Object.assign(Point.prototype, {
  // 这两个就是给Point新增的动态方法
  toString(){
    // ...
  }, // 别忘了这里的逗号
  toValue(){
    // ...
  }
});
```

# 类里的方法名可以采用表达式

class里的方法名可以使用计算值，这是构造函数做不到的。

```javascript
let a = 'toString';

class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  [a]() { // 把表达式用[]包起来就可以了
    return this.x + this.y;
  }
}

var b = new Point(10, 20);
console.log(b.toString()); // 30
```

# 注意严格模式

class内部是默认使用严格模式的，所以，最好你就从今天开始养成js代码全局都使用严格模式的习惯，只有好处没有坏处。

# class默认自带constructor方法

constructor方法是类的默认方法，它的作用是：通过new命令生成对象实例时，自动调用该方法。通常类的动态属性都在constructor方法中初始化。

如果没有显式定义constructor方法，则class会默认创建一个空的constructor方法。

# class的表达式写法

跟函数一样，class也可以用表达式写法，下面栗子中，当前作用域下，这个类叫`PointClass`，但是类的内部，这个类叫`Point`。

表达式写法中，`Point`可以省略，跟函数一样。只要类的内部不需要用到`Point`，就可以省略。

```javascript
const PointClass = class Point {
  constructor(x, y) {
    Point.x = x;
    Point.y = y;
  }

  toString() {
    return Point.x + Point.y;
  }
}

var a = new PointClass(10, 20);
console.log(a.toString()); // 30
```

# 类的声明，不存在变量提升

我们知道function是有变量提升特性的：

```javascript
a();

function a() {
  console.log('b'); // 会被执行
}
```
而class是没有变量提升特性的。记住这一点。这种规定的原因与下文要提到的继承有关，必须保证子类在父类之后定义。

# 私有方法、私有属性

ES6依然没有实现其他语言早已实现的私有方法和私有属性。只能变通实现：

有一种方法是利用Symbol值的唯一性，将私有方法的名字命名为一个Symbol值。具体需要先了解Symbol，本文略过。

# class也有name属性
name属性是class默认自带属性，它就是返回class的名字，就是跟在class后面的那个类名。

# class的取值函数(get)和存值函数(set)
在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。
为什么会有这种函数呢？看个栗子。我设一个班作为一个类，这个class有一个人数属性，下面例子中，我new的时候设定初始人数是60人。然后，某时间，班里转入新生30人，我想知道现在多少人？然后过了一阵，又转入20人，我想知道现在多少人，怎么写代码？

```javascript
class MyClassroom {
  constructor(number) {
    this.number = number;
  }
  get newnumber() {
    return this.number;
  }
  set newnumber(value) {
    this.number += value;
  }
}

var classroom = new MyClassroom(60);
classroom.newnumber = 30;
console.log(classroom.newnumber); // 90
classroom.newnumber = 20;
console.log(classroom.newnumber); // 110
```

说白了，get和set关键字，一个负责取值，一个负责存值，如果不这样做，我还能怎么写呢？

```javascript
class MyClassroom {
  constructor(number) {
    this.number = number;
  }
  getnewnumber() {
    return this.number;
  }
  setnewnumber(value) {
    this.number += value;
  }
}

var classroom = new MyClassroom(60);
classroom.setnewnumber(30);
console.log(classroom.getnewnumber()); // 90
classroom.setnewnumber(20);
console.log(classroom.getnewnumber()); // 110
```

这样我不用get和set关键字，而是定义了2个方法，也达到了同样的目的。

所以get和set关键字的优势是什么？
1. 简练。如果不用get、set关键字，那么就需要写2个方法名，这样显然不够简练。原本早期阶段编程语言种确实没有get、set概念，那时候程序员们的确经常用getnewnumber和setnewnumber这种方法来定义方法，后来语言的维护者看到既然大家都喜欢这么写，就干脆提出了get和set两个关键字，这样就更精炼了。
2. 直观。 `classroom.newnumber = 30`是一种更直观的赋值写法，比`classroom.setnewnumber(30)`这种传参的写法要直观的多。
3. 处理流程分离。get要做的事情跟set要做的事情完全不用。
4. 最重要的一点：安全。虽然ES6还没有实现私有属性和方法，但是其他语言早就实现了，既然私有，意味着不能直接写入和读取，那么怎么办呢？就用get和set作为对外窗口，来统一接收数据和发出数据。当某个属性只允许写，那就只设set没有get，只允许读，就只设get没有set。
5. 操作拦截，其实也是为了安全。如果让你随意修改属性，比如班级人数，没有set的话，你修改number属性就直接修改了人数，怎么可以让你这么轻松的修改呢？因为你如果修改成一个很扯淡的人数呢？或者你不小心把布尔值当数值传进去又该咋办？现在，有set函数，于是所有的类型判断、范围判断、有效性判断，都可以加到里面，全OK的情况下，最终才允许你修改成功。

# class的Generator方法

这个Generator词，跟ES6引入的Generator函数的Generator是一个意思。
如果class的某个方法之前加上星号（*），就表示该方法是一个 Generator 函数。class结合Generator比较复杂，外部需要`for (... of ...) {}`来处理实例，如果真的需要的时候，可以再去查专门的资料，这里不多说。

# class内部无法定义自身静态属性，只能在外部定义

需要注意，class内部的this关键字，指向的是实例，并不是类本身。

在class的内部定义静态属性是非法的：

```javascript
class MyClass {
  a: 1 // Uncaught SyntaxError: Unexpected token :
}
```

所以，定义类自身的静态属性， 只能是在外部定义：

```javascript
class MyClass {
}
MyClass.a = 1;
console.log(MyClass.a); // 1
```

# class可以定义实例属性但必须定义在构造方法里

目前ES6的规定就是如此，因为class毕竟只是语法糖。

栗子从略，可参看本文任意构造方法。
# class可以定义类自身方法（即静态方法）

类自身的方法，叫做静态方法，做法是在class的一个方法前，加上static关键字。静态方法不会被实例继承，而是直接通过类来调用。

一定要区分class的方法和实例的方法。

```javascript
class MyClassroom {
  constructor(number) {
    this.number = 60;
  }
  static get1() {
    return this.number; 静态方法包含的this关键字，这个this指的是类，而不是实例
  }
   static get2() {
    return 80;
  }
}

console.log(MyClassroom.get1()); // undefined
MyClassroom.number = 60;
console.log(MyClassroom.get1()); // 60
console.log(MyClassroom.get2()); // 80

var classroom = new MyClassroom();
console.log(classroom.get1()); // undefined
```
静态方法可以与非静态方法重名。

虽然实例无法继承静态方法，但是类的子类可以继承静态方法：

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello' 父类Foo有一个静态方法，子类Bar可以调用这个方法。
```
静态方法也是可以从super对象上调用。具体参考下文关于继承的知识。

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}

Bar.classMethod() // "hello, too"
```

# new.target用于实现只允许继承，不允许调用的class

`new.target`必须写在构造方法里面，它指向类本身。具体指向哪个类，有下面说道：

类没有被继承的话，`new.target`就指向类自身。
类被继承的话，`new.target`指向子类。

`new.target`的用途主要是确定new的对象到底是以哪个类为范本。实践中，基类是不允许被实例化的，靠这个`new.target`就可以实现基类禁止被实例化。

```javascript
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```



