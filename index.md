# INDEX
1. [函数](#函数)
2. [作用域](#作用域)
3. [js预解析](#js预解析)
4. [js对象](#js对象)
5. [js内置对象](#js内置对象)
6. [简单数据类型和复杂数据类型](#简单数据类型和复杂数据类型)


## js代码引入

行内引入

```html
  <script src="index.js"></script>
```

---

## 函数

#### 两种声明方式

##### 有名函数

##### 匿名函数
```js
var fun = function(n, ... set) {
  for (var i = 0; i <= arguments; i++)document.write(arguments[i]);
}

console.log(typeof(fun)); // function

```

## 作用域

 + 全局作用域
   + js文件
 + 局部作用域（即函数作用域）
   + 函数内部

#### 变量作用域

```js
var n = 10;
console.log(n); // 全局

var f = function (n) {
  console.log(n);
}
f(0); // 局部
```

**全局变量只有浏览器关闭时才会销毁，比较占内存，执行效率不如局部变量**

#### *js没有块级作用域

es6新增了块级作用域，c语言也有块级作用域

#### 作用域链

内部函数访问外部变量采取链式查找

```js
var num = 10;
    function a() {// 外部函数
      console.log(arguments[0]); // 输出10
      var num = 100;
      function b() {// 内部函数
        console.log(arguments[0]);
      }
      b(num); // 链式查找上一级num，输出100
    }
    a(n);
```


## js预解析

- js 引擎运行 js 分两步
  - 预解析
    - js 引擎会把 var 和 function 提到当前作用域最前面
      - 变量与解析（变量提升）：只提升声明不提升赋值
      - 函数与解析（函数提升）: 把所有函数声明提升到当前作用域最前面，不调用函数
  - 代码执行
    - 从上往下依次执行
```js
document.write(n); // undefined
    var n = 10;

    f(10); // 10
    function f(n) {
      document.write(n);
    }

    fun(20); // fun is not a function
    var fun = function (n) {
      document.write(n)
    }
```

> 预解析后的代码

```js
// 变量提升
var n;
document.write(n); // undefined
n = 10;

// 函数提升
function f(n) {
  document.write(n);
}
f(10); // 10

// 变量提升
var fun;
fun(20); // fun is not a function
fun = function (n) {
  document.write(n)
}
```

具体案例

```js
var n = 10;
function f() {
  console.log(n); 
  var n = 100;
  console.log(n); 
}
f();
```

> 预解析

```js
var n; // 变量提升
function f() { // 函数提升
  var n;
  console.log(n); // 变量提升，输出undefined
  n = 100;
  console.log(n); // 链式查找，输出100
}

n = 10;
f();
```

```js
var a = b = c = 9;
// 相当于 b 和 c 没有声明直接赋值，当全局变量看
var a;a = 9;b = 9;c = 9;
// 集体声明：
var a = 9, b = 9, c = 9;
```
案例
```js

    f();
    console.log(c);
    console.log(b);
    console.log(a);

    function f() {
      var a = b = c = 9;
      console.log(a);
      console.log(b);
      console.log(c);
    }
    // 输出 五个9，a is not defined
```
 ---
# js对象

 + 对象是一个具体的事物
 + js中的对象是一组无序的相关属性和方法的集合
 + 字符串，数值，数组，函数都是对象 

## 字面量创建，使用对象

- 创建对象
  - 对象里的属性和方法声明采用键值对的方式
  - 多个属性，方法之间用```,```隔开
  - 方法后跟一个匿名函数
- 使用对象
  - 调用属性 `对象名.属性` 或 `对象名['属性名']`
  - 调用方法 `对象名.方法名`



```js
var obj = {
  uname: "o",
  age: 100,
  sex: 0;

  sayHi: function() {
    console.log("Hi");
  }
}

console.log(obj.uname);
obj.sayHi();
```

## `new`关键字创建变量

- 用赋值`=`添加对象的属性和方法
- 每个属性和方法之间用`;`结束
  

```js
var obj = new Object(); // 创建了一个空对象
obj.uname = "Eee";
obj.sayHi = function() {
  console.log();
}
```

## 构造函数创建对象

- 构造函数相当于一个类
- 创建对象的过程称为实例化

```js
function 构造函数名() {
      this.属性 = 值;
      this.方法 = function() {

      }
    }
new 构造函数名();
```

- 构造函数名首字母大写
- 构造函数不需要返回值
- 调用构造函数必须用`new`
- `new People()`调用构造函数
就创建一个对象 `obj{}`
```js
 function People(uname, age, sex) {
      this.name = uname;
      this.age = age;
      this.sex = sex;
      this.say = function() {
        console.log(1);
      }
    }

    var Eee = new People("Eee", 100, 1);
    console.log(typeof Eee); // object
```

> 王者荣耀英雄案例

```js
function Hero(Name, Type, Hp, Exp, attack) {
  this.name = name;
  this.Type = Type;
  this.Hp = Hp;
  this.Exp = Exp;
  this.attack = attack;
  this.LevelUp = function () {
    this.Exp++;
  }
}

var 后裔 = new Hero("后裔", "射手", 3200, 1, 150);

// 1级 -> 2级
console.log(后裔.Exp);
后裔.LevelUp();
console.log(后裔.Exp);
```

## `new`关键字执行过程

1. 在内存中创建一个空对象
2. `this` 指向这个空对象
3. 执行构造函数力的代码，给空对象添加属性和方法
4. 返回这个对象（`new`会返回，所以不用`return`）

### 遍历对象属性

`for...in`遍历
```js
for (var k in obj) {
  console.log(k); // 键
  console.log(obj[k]); // 值
}
```


--- 
# js内置对象

- TARGET
  - [js对象分类](#js对象分类)
  - [会查文档看指定API](#查文档)
  - [会使用Math对象](#使用Math对象)
  - [会使用Date对象](#使用Date对象)
  - [会使用Array对象](#使用Array对象)
  - [会使用String对象](#使用String对象)

## js对象分类

- 自定义对象
- 内置对象
- 浏览器对象

## 查文档
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

## 使用Math对象
+ Math数学对象不是函数对象，不用`new`进行创建，可以直接使用
+ 
## 使用Date对象

## 使用Array对象

## 使用String对象

# 简单数据类型和复杂数据类型
> 简单数据类型又叫基本数据类型，值类型。
> 复杂数据类型又叫引用类型

## 简单数据类型内存分配

### 堆，栈

- 简单数据类型放在栈里
- 复杂数据类型放在堆里
- 
简单数据类型：`string`,`boolean`,`number`,`undefined`,`null`

简单数据类型`null`返回的是一个空对象`object`
```js
var timer = null;
console.log(typeof timer);
```


## 复杂类型内存分配

## 

> todo 1 : 内置对象
[> todo 2 : 内存分配](https://www.bilibili.com/video/BV1Sy4y1C7ha?p=188&spm_id_from=pageDriver&vd_source=cbeee4de613104de8bf0753a8c1feccc)