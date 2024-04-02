---
layout: post
title: 【C#学习】31委托，Lambda表达式，LINQ
category: Csharp
date:  最新推荐文章于 2020-01-30 15:30:39 发布
---

#### 文章目录

- [委托](#委托)
  - [1.什么是委托？](#1什么是委托)
  - [2.怎么使用委托？](#2怎么使用委托)
  - [3.泛型委托](#3泛型委托)
- [Lambda表达式](#lambda表达式)
  - [1.方法与Lambda表达式之间的关系](#1方法与lambda表达式之间的关系)
  - [2.把一个Lambda表达式赋值给一个委托类型的变量](#2把一个lambda表达式赋值给一个委托类型的变量)
  - [3.把一个Lambda表达式"喂"给一个委托类型的参数](#3把一个lambda表达式喂给一个委托类型的参数)
- [LINQ](#linq)


## <a id="_1"></a>委托

### <a id="1_2"></a>1.什么是委托？

委托是类类型，是一种特殊的类，它表现在：<br> （1）功能特殊：不是反映现实事物，而是 “包裹” 着一些方法，通过委托实例【间接调用】方法；委托是方法的封装器/包装器；程序上下文固定，但在某个关键部分，调用哪个函数是不确定的，又不想让函数之间产生紧耦合关系，就可用委托来进行间接/可替换调用

（2）声明方法特殊

![20200130184721962.png](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/20200130184721962.png)

### <a id="2_10"></a>2.怎么使用委托？

![20200130184738118.png](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/20200130184738118.png)

### <a id="3_13"></a>3.泛型委托

（1）Action&lt;&gt;委托，Func&lt;&gt;委托<br> 见上节

（2）自定义泛型委托
![](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/20240402112339.png)
![20200130184804797.png](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/20200130184804797.png)



## <a id="Lambda_20"></a>Lambda表达式

### <a id="1Lambda_21"></a>1.方法与Lambda表达式之间的关系

Lambda表达式声明：<br> （1）匿名方法<br> （2）InLine方法：在调用的时候才去声明的方法（随调用，随声明）

### <a id="2Lambda_26"></a>2.把一个Lambda表达式赋值给一个委托类型的变量

<mark>把一个Lambda表达式赋值给一个委托类型的变量</mark>，是未来**非常常用的语法**，可理解为Lambda表达式求值完之后是一个委托类型的实例<br> ![20200130184815137.png](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/20200130184815137.png)

### <a id="3Lambda_31"></a>3.把一个Lambda表达式"喂"给一个委托类型的参数

函数的形参本身也是一种变量，如果一个函数具有委托类型的参数，在调用该方法时，是否可以把Lambda表达式作为实参进去呢？<br> 当然可以<br> ![20200130184828777.png](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/20200130184828777.png)

## <a id="LINQ_37"></a>LINQ

LINQ：.NET Language Integrated Query

- Language：指C#，VB，F#等
- Query：查询（多是数据库）
- Integrated：在没有LINQ之前，如果想要查询数据库，必须要用SOL sever代码，不会就去学；有了LINQ之后，程序员在进行一些不是很复杂的数据库查询操作时（不止数据库查询），就不必再去专门学习SOL sever，运用LINQ，在程序运行时，它会把查询逻辑所用的 C#代码 翻译成 SQL sever 代码，然后传到远端的SQL中，把查询出来的东西形成一组对象，让我们来操作对象，这个过程就是 Integarted
但是利用LINQ性能会有所降低，所以还是很建议掌握 SQL sever
