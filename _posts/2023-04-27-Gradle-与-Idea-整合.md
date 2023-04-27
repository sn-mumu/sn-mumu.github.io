---
title: Gradle 与 Idea 整合
author: mumu
date: 2023-04-27 14:30:00 +0800
categories: [Gradle,Gradle 与 Idea 整合]
tags: [Gradle,Gradle 与 Idea 整合,Groovy]
pin: false

---

# Gradle 与 Idea 整合[^1]

## 1 Groovy 简介  

在某种程度上，Groovy 可以被视为 Java 的一种脚本化改良版,Groovy 也是运行在 JVM 上，它可以很好地与 Java 代码及其相关库进行交互操作。它是一种成熟的面向对象编程语言，既可以面向对象编程，又可以用作纯粹的脚本语言。大多数有效的 Java 代码也可以转换为有效的 Groovy 代码，Groovy 和 Java 语言的主要区别是：<font color='' style='background-color:yellow' size=''>完成同样的任务所需的Groovy 代码比 Java 代码更少</font>。

### 1.1. Groovy特点

1. 功能强大，例如提供了动态类型转换、**<font color='cornflowerblue' style='background-color:' size=''>闭包</font>** 和元编程（metaprogramming）支持
2. <font color='cornflowerblue' style='background-color:' size=''>支持函数式编程</font>，不需要 main 函数
3. 默认导入常用的包
4. 类不支持 default 作用域,且默认作用域为 public。
5. <font color='cornflowerblue' style='background-color:' size=''>Groovy 中基本类型也是对象，可以直接调用对象的方法</font>。
6. 支持 DSL（Domain Specific Languages 领域特定语言）和其它简洁的语法，让代码变得易于阅读和维护。
7. Groovy 是基于 Java 语言的，所以完全兼容 Java 语法,所以对于 java 程序员学习成本较低。

详细了解请参考：[Apache Groovy](http://www.groovy-lang.org/index.html)

## 2 Groovy 安装<font color='cornflowerblue' style='background-color:' size=''> ^△^ </font> 

1. 下载 

   [Download](https://groovy.apache.org/download.html)

2. 系统环境变量的配置

   ![image-20230427144411898](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/202304271444011.png)

## 3 创建 Groovy 项目  

![image-20230427145122601](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/202304271451696.png)

## 4 Groovy 基本语法  

1. Groovy是基于Java语言的，所以完全兼容Java语法，可作为<font color='red' style='background-color:' size=''>面向对象编程语言</font> [定义类]，也可作为<font color='red' style='background-color:' size=''>脚本型语言</font> [文件定义中不出现类]

2. 在一个gooy文件中<font color='red' style='background-color:' size=''>可以混合类的定义和脚本定义</font>。此时不要再定义一个和文件同名的类。

3. groovy中使用<font color='red' style='background-color:' size=''>def定义变量、方法</font>，不建议使用具体的数据类型

4. groovy中的注释：`单行注释用∥，多行注释用：/* */`

5. Groovy中<font color='red' style='background-color:' size=''>语句末尾的分号是可以省略的</font>，以换行作为结束。

6. <font color='red' style='background-color:' size=''>默认类、方法、字段都是publicf修饰的</font>

7. 对象的属性操作

   + 给对象属性赋值赋值

     + 对象.属性名=XXx

     + 对象的setter方法

     + 具名构造器【groovy类自带的】

       用处：后面给task指定分组，description描述信息使用

   + 对象属性读取操作

     + 对象.属性名
     + 对象["属性名"]
     + 对象.getter方法

   + 对类属性的操作本质是通过属性对应的getter、setter方法完成的

8. 方法

   + 声明时
     + 参数类型、返回值类型可省略
     + return关键字：默认使用方法最后一句的返回值作为方法返回值

9. 支持 `顺序结构、分支结构、循环结构`语句

10. 支持的各种运算符：`算术、关系、位、赋值、范围`运算符

11. 基本类型也是对象，可以直接调用对象的方法

    + groovy 中的字符串有单引号、双引号、三个引号
      + 单引号：作为字符串常量使用，没有运算能力
      + 双引号：可引用变量`${}`，有运算能力
      + 三引号：模板字符串，支持换行

12. <font color='red' style='background-color:' size=''>数据类型：变量，属性，方法，闭包的参数以及方法的返回值的类型都是可有可无的，都是在给变量赋值的时候才决定它的类型</font>

13. 类型转换：当需要时,类型之间会自动发生类型转换: 字符串（String）、基本类型(如 int) 和类型的包装类 (如 Integer)

14. 类说明：如果在一个 groovy 文件中没有任何类定义，它<font color='red' style='background-color:' size=''>将被当做 script 来处理</font>，也就意味着这个文件将<font color='red' style='background-color:' size=''>被透明的转换为一个 Script 类型的类</font>，这个自动转换得到的类将使用原始的 groovy 文件名作为类的名字。groovy 文件的内容被打包进run 方法，另外在新产生的类中被加入一个 main 方法以进行外部执行该脚本  

### 4.1. 案例分析

![image-20230427151055255](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/04/202304271510358.png)

方法调用时,在不含有歧义的地方可以省略方法调用时的括号。这类似于使用`${变量名}`时，括号在不引起歧义的地方可以省略是一样的  

```groovy
def num1=1;
def num2= 2;
println "$num1 + $num2 = ${num1+num2}"
```

### 4.2. 引号说明  

```groovy
def num1=1;
def num2=2;
def str1="1d"; //双引号
def str2='dsd'; //单引号
//双引号运算能力,单引号用于常量字符串,三引号相当于模板字符串，可以支持换行
println "$num1 + $num2 = ${num1 + num2}"
//基本数据类型也可以作为对象使用,可以调用对象的方法
println(num1.getClass().toString())
println(str1.getClass().toString())
println(str2.getClass().toString())
```

### 4.3. 三个语句结构  

Groovy 支持顺序结构从上向下依次解析、分支结构(if..else、if..else if ..else..、switch..case、for、while、do..while)
具体参考官网：http://www.groovy-lang.org/semantics.html#_conditional_structures  

### 4.4. 类型及权限修饰符  

#### 4.4.1. 类型

1. 原生数据类型及包装类  

   | Primitive type | Wrapper class |
   | -------------- | ------------- |
   | boolean        | Boolean       |
   | char           | Character     |
   | short          | Short         |
   | int            | lnteger       |
   | long           | Long          |
   | float          | Float         |
   | double         | Double        |

2. 类、内部类、抽象类、接口  

3. 注解  

4. `Trait`: 可以看成是带有方法实现的接口  

5. <font color='red' style='background-color:' size=''>权限修饰符  </font>

   public、protected、private  

#### 4.4.2. Groovy 类与 Java 类之间的主要区别  

1. 没有可见性修饰符的类或方法<font color='red' style='background-color:' size=''>自动是公共</font>的(可以使用一个特殊的注释来实现包的私有可见性)。
2. 没有可见性修饰符的字段将自动转换为属性，不需要显式的 getter 和 setter 方法。
3. 如果<font color='red' style='background-color:' size=''>属性声明为 final，则不会生成 setter</font>。
4. 一个源文件可能包含一个或多个类(但是如果一个文件不包含类定义的代码，则将其视为脚本)。脚本只是具有一些特殊约定的类,它们的名称与源文件相同(所以不要在脚本中包含与脚本源文件名相同的类定义)。  

有关Groovy中各种各样的数据类型和权限修饰符及Goovy与Java区别请参考：http://www.groovy-lang.org/objectorientation.html#_modifiers_on_a_property  

### 4.5. 集合操作  

> Groovy 支持 List、Map 集合操作，并且拓展了 Java 中的 API  
>
> **<font color='red' style='background-color:' size=''>可以把不同的基本类型添加到同一集合中</font>**  

#### 4.5.1. List  

+ add():添加某个元素
+ plus():添加某个 list 集合
+ remove():删除指定下标的元素
+ removeElement():删除某个指定的元素
+ removeAll(): 移除某个集合中的元素
+ pop():弹出 list 集合中最后一个元素
+ putAt():修改指定下标的元素
+ each():遍历
+ size(): 获取 list 列表中元素的个数
+ contains(): 判断列表中是否包含指定的值，则返回 true  

#### 4.5.2. Map  

+ put():向 map 中添加元素
+ remove():根据某个键做移除，或者移除某个键值对
+ `+、-`：支持 map 集合的加减操作
+ each():遍历 map 集合  

请参考官网:http://www.groovy-lang.org/syntax.html#_number_type_suffixes  

### 4.6. 类导入  

> Groovy 遵循 Java 允许 import 语句解析类引用的概念

```groovy
import groovy.xml.MarkupBuilder
def xml = new MarkupBuilder()
assert xml != null
```

Groovy 语言<font color='red' style='background-color:' size=''>默认提供的导入</font>  

```groovy
import java.lang.*
import java.util.*
import java.io.*
import java.net.*
import groovy.lang.*
import groovy.util.*
import java.math.BigInteger
import java.math.BigDecimal
```

*这样做是因为这些包中的类最常用。通过导入这些样板代码减少了*  

参考官网地址：http://www.groovy-lang.org/structure.html#_imports  

### 4.7. 异常处理  

> Groovy 中的异常处理和 java 中的异常处理是一样的  

```groovy
def z
try {
    def i = 7, j = 0
    try {
        def k = i / j
        assert false
    finally {
        z = 'reached here'
    }
} catch ( e ) {
    assert e in ArithmeticException
    assert z == 'reached here'
}
```

参考官网地址： http://www.groovy-lang.org/semantics.html#_try_catch_finally  

### 4.8. 闭包  































[^1]: [尚硅谷](http://www.atguigu.com/) ↩