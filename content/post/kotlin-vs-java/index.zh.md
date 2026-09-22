---
title: "Kotlin 和 Java 语法比较"
description: 比较 Kotlin 和 Java 语法层面的主要差异
date: 2026-08-24
categories:
    - 编程
tags:
    - kotlin
math: false
comments: false
draft: false
build:
    list: always
---

## Kotlin VS. Java

### 类型

- 数据类型

  Kotlin 中没有 Java 中的基本数据类型，都是封装类型。Kotlin 编译器会根据实际需要编译成基本类型或封装类型。

- 可空类型

  Kotlin 中的类型区分**可空**和**非空**，默认是非空，不能赋 null 值。可空类型在类型后增加`?`，如：`String?`

- 只读变量

  Kotlin 变量声明时使用 `var` 或 `val`

- 数据类型的位置

  Kotlin 声明变量时数据类型放在后边，如：`val i: Int`；函数返回值也放在后边，如：`fun test(i: Int): Boolean`

- 类型转换

  Kotlin 中的数值类型之间没有隐式的类型转换，每种数据类型都有转换为其他类型的 to***() 方法，如 Int 上的 `toLong()`, `toShort()` 等方法。

### 运算符

- 逻辑运算符

  Kotlin 中的 `==` 用于判断对象的内容是否相等，相当于 Java 中的 `equals()` 方法（自定义类需要重写 equals 函数）。Kotlin 增加了 `===` 用于引用判等。

- 位运算符

  Kotlin 中没有 Java 中的 `&`, `|` 等位运算符，而是用 `and`, `or` 这样的中缀函数

- 辅助解决 NullPointerException 问题的方式

  Java 增加了 `Optional` 类，Kotlin 增加了安全调用运算符 `?.`

- 三元运算符

  Kotlin 中没有 Java 中的三元运算符 `? :`

  Kotlin 提供了 if else 表达式

  Kotlin 中提供了 `?:` 运算符，`A ?: B` 表示当 A 为 null 时结果为 B，否则结果为 A

### 结构

- 多分支结构

  Kotlin 提供了 `when` 语句代替 Java 的 switch 语句

- 语句表达式

  Kotlin 中的结构语句可以作为表达式，如 if - else, when, try 等等

- for 循环

  Kotlin 中没有 Java 的 for(i=0; i<10; i++) 这样的 for 循环，for 只能以 for each 的方式使用

- label

  Kotlin 中 label 的写法不同于 Java，而是用 `label@` 表示，break 和 continue 搭配标签的写法为 `break@label`, `continue@label`

- Range

  Kotlin 提供了生成区间的写法：

  - `i in 0..5`: [0, 5]
  - `i in 0 until 5`: [0, 5)
  - `i !in stringArray`

### 函数

- 函数

  函数在 Kotlin 中是**一等公民**，可以作为参数、返回值使用。

- void

  Kotlin 中没有 void，函数如果没有返回值，需要声明成 `Unit`（可以省略）

- 默认参数

  Kotlin 中函数的参数可以设置默认值，这种方式在一定程度上可以代替 Java 中的方法重载：

  `fun test(s: String = "abc"): Boolean`

- 参数传递

  Kotlin 中向函数传递参数时，可以像 Java 一样按参数声明的顺序传递，也可以按名称指定，此时参数的顺序无关。

  还可以前边按顺序，后续按名称（一旦其中一个参数采用了命名参数形式传递，其后所有参数都必须采用命名参数形式传递）

- 变长参数

  Kotlin 中的变长参数需要用 `vararg` 指定

- 表达式函数体

  如果函数体中的表达式能够表示成单个表达式时，那么函数可以简化为表达式函数体：`fun area(width: Double, height: Double) = width * height`

  表达式函数体省略了大括号和 return 语句，直接返回表达式，还可以省略函数返回类型。

- 中缀运算符

  Kotlin 支持中缀运算符。中缀运算符本质上是一个**函数**。定义中缀运算符，需要声明一个 `infix` 关键字修饰的函数，该函数只能有一个参数，且该函数不能是顶层函数，只能是成员函数或扩展函数。使用时可以不使用函数调用方式（`obj.test(param)`），而是中缀运算符方式（`obj test param`）

### 类

- 类

  Kotlin 的类**默认是封闭的**，不允许继承。如需继承，需要将类声明为 `open`。Java 的类默认是可继承的，如不可继承，需要声明为 final。

  类的属性和函数如果需要被重写，也需要声明为 `open`。子类如果重写了父类的属性或函数，需要将属性或函数声明为 `override`。

  Kotlin 并不要求类名与文件名一致，且一个文件中不限制类的数量。

  Kotlin 所有对象的共同基类是 `Any`，而不是 Java 中的 Object。

- 构造实例

  Kotlin 通过调用类的构造函数创建类的实例，**不需要 new**。

- 类的可见性

  Kotlin 类的可见性分为 `public`（默认）, `internal`, `protected`, `private`, 没有 Java 中的"包可见"

- 类的属性

  Kotlin 类的每个属性，编译器都会生成一个支持字段（backing field，存储属性数据，用 `field` 访问）、一个getter、一个setter（var 属性有，val 属性没有）。getter 和 setter 是自动提供的，可以自行修改（重写 `get()` 和 `set()` 方法）

- 类的构造函数

  Kotlin 类的构造函数分为**主构造函数**和**次构造函数**。主构造函数只有一个，次构造函数个数不限。

  主构造函数在类名后的"()"中声明：`class User(name: String)`。如果参数中带有 `var` 或 `val`，则该参数自动成为类属性。

- Kotlin 主构造函数的初始化代码在 `init()` 方法中定义

- Kotlin 类的属性可以通过 `lateinit` 声明为延迟初始化

- Kotlin 类的属性可以通过 `by` 将 getter 和 setter 委托给其他函数提供，例如：`by lazy` 实现惰性初始化

- Kotlin 对象类型检测通过 `is`，而不是 Java 的 instanceof。

- Kotlin 对象的强制类型转换通过 `as`，如：`user as Person`，而不是 Java 的 "(Person) user"。

- Kotlin 支持**操作符重载**：例如 List 对象可以通过 [] 操作符替代 get 函数

  操作符重载需要用 `operator` 关键字声明函数。例如重载 "+" 运算符：`operator fun plus(...) {...}`

- Kotlin 接口支持定义属性，但不能有构造函数

### 泛型

- Kotlin 泛型参数支持 `out`, `in` 修饰符，分别表示协变（只能用于返回值）、逆变（只能用于入参）

- Kotlin 泛型参数支持 `reified` 修饰符，保留泛型参数类型。但 reified 必须与 `inline` 一同使用

### 扩展

- 除继承外，Kotlin 中可以扩展原始类型的函数和属性，原始类型称为"**接收类型**"。扩展必须针对某种接收类型，所以**顶层函数和属性没有扩展**。

