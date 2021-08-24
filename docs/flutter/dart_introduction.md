---
title: Flutter基础之Dart语言入门
date: 2019-12-09 10:17:27
tags: 
    - flutter
category:
    - flutter
---


> Dart是Flutter开发语言，学习一门技术，首先要从开发语言开始。本篇开始从开发语言开始，目的是为0基本的朋友能更方便的了解这门开发语言，同时有开发基本的也可以作为笔记查看。

#### 语言特性
Dart官网：http://www.dartdoc.cn

Dart是一门面向对象的开发语言，所有的对象都继承自Object类， 包括数字numbers、函数function、null也都是对象。

Dart和Object-C一样也具有动态类型语言特性, 尽量给变量定义一个类型，会更安全，没有显示定义类型的变量在 debug 模式下会类型会是 dynamic(动态的)。

Dart 在 running 之前解析你的所有代码，指定数据类型和编译时的常量，可以提高运行速度。

Dart中的类和接口是统一的，类即接口，你可以继承一个类，也可以实现一个类（接口），自然也包含了良好的面向对象和并发编程的支持。

Dart 提供了顶级函数(如：main())，俗称入口函数。

Dart 和java不一样，没有 public、private、protected 这些关键字，变量名以"_"开头意味着对它的 lib 是私有的。

没有初始化的变量都会被赋予默认值 null。


编程语言并不是孤立存在的，Dart也是这样，他由**语言规范**、**虚拟机**、**类库**和**工具**等组成：

* SDK：SDK 包含 Dart VM、dart2js、Pub、库和工具。
* Dartium：内嵌 Dart VM 的 Chromium ，可以在浏览器中直接执行 dart 代码。
* Dart2js：将 Dart 代码编译为 JavaScript 的工具。
* Dart Editor：基于 Eclipse 的全功能 IDE，并包含以上所有工具。支持代码补全、代码导航、快速修正、重构、调试等功能。


#### 运算符

![运算符](/image/dart_introduction/%E8%BF%90%E7%AE%97%E7%AC%A6.jpg)


新运算符

```dart
..（级联运算符）和 ?.（条件成员访问运算符）以及 ??（判空赋值运算符)
?.   如 Test?.funs 从表达式Test中选择属性funs，除非Test为空（当Test为空时，Test?.funs的值为空)

as  类型转换 (确定是指定类型时才可以使用as转换类型)
is  如果对象具有指定的类型，则为true
is! 对象不是某个类型
```



#### 变量与常量

**var**
Dart是强类型语言. 当var声明一个变量后，Dart在编译时会根据第一次赋值数据的类型类推断其类型。编译完成后其类型就已经被确定。 

```dart
Dart中的var变量一旦赋值，类型遍会确定，则不能再改变其类型。

var t;
t="hi world";
// 下面代码在dart中会报错，因为变量t的类型已经确定为String，
// 类型一旦确定后则不能再更改其类型。
t=1000;
```

```dart
整型

num(int , double) 
运算符  / (除完的结果是浮点型)   ~/ 取整(除完后取整)
```

**字符串**

```dart
String str = ‘ Hello’
print(str * 5) // 字符串变量 * 5 表示把变量值拼接了5次返回 

```

```dart
1. contains()
2. cusString()
3. indexOf()
4. lastIndexOf()
5. toLowerCase()
6. toUpperCase()
7. trim()
8. trimLeft()
9. trimRight()
10. split()
11. replaceXXX()
...
```


**三个单引号** 或 **三个双引号** 

```dart
String str = '''Hello
                  Dart''';
                  
String str = """Hello
                  Dart"””;

```

使用 **r** 创建原始字符串

```dart
String str = r'Hello \n Dart'; // "\n"不会被转义
```


**单引号里面嵌套单引号**，或者**双引号里面嵌套双引号**，必须在前面加反斜杠

![F2ED24C2-F022-4922-8240-B3E7C5FE638E](/image/dart_introduction/F2ED24C2-F022-4922-8240-B3E7C5FE638E.png)


**List(数组)**

```dart
创建List：var list[1,2,3];
创建不可变的List：var list = const [1,2,3];
构造创建：var list=new  List();
常用操作
```


**Map 字典**

```dart
创建不可变的map
List：var map = const {1:”123”,2:”456”};
```

**赋值运算符**

```dart
??=  
b ??= 100; // 表示左侧变量为空时进行赋值，否则不赋值
```

**表达式**

```dart 
?? 
运算符 expr1 ?? expr2// 意思第一个表达式expr1不为空，则直接使用expr1，如果为空，则使用expr2；
```

**插值表达式** : ${expression}

使用 ${ } 表示插件表达式，单个变量可省略 { }。

```dart
int a = 1;
int b = 2;
print(" a + b = ${a + b} "); // 输出 a + b = 3
print(“ a = $b”); 等同于 print(“ a = ${b}”);  // 可以省略大括号
```


**可选参数**

```dart
可选参数基于名称{}
可选命名参数：{param1,param2,...}

可选参数基于位置[]
可选命名参数：[param1,param2,...]
```


**dynamic** 和 **Object **
Dynamic和Object与var功能相似，都会在赋值时自动进行类型推断，不同在于，赋值后可以改变类型。

```dart 
dynamic t;
t="hi world";
//下面代码没有问题
t=1000;
```


**final** 和 **const**
如果未打算更改一个变量，那么使用final 或 const，不是var ， 也不是一个类型，
一个final变量只能被设置一次， 两者区别在于：const变量是一个编译时常量，final变量在第一次使用时被初始化，被final或const修饰的变量，变量类型可以省略，类型根据值而定，如：

```dart
//可以省略String这个类型声明
final str = "hi world";
//final String str = "hi world"; 
const str1 = "hi world";
//const String str1 = "hi world";
```

final 的值只能被设定一次。
const 是一个编译时的常量，可以通过 const 来创建常量值，var c=const[];，这里 c 还是一个变量，只是被赋值了一个常量值，它还是可以赋其它值。实例变量可以是 final，但不能是 const。


**级联操作**

```dart
// 使用 .. 级联操作 可对同一对象执行一系列操作
Dio()
..options.baseUrl = 'http://app4.jinriaozhou.com/'
..options.connectTimeout = 500
..options.receiveTimeout = 300;
```




