title: Java8 Functional Programming
date: 2018-09-02 14:54:53
tags: [coding,summary, java, functional programming]
categories:  Java 
description: Java8 Lambda: Functional Programming Summary.
---

## Introduction

- 在Java8之前，java代码的抽象程度不够，同时还欠缺高效的并行操作。
- Java8中引入了lambda表达式，面向对象编程可以看作是对数据进行抽象，而函数式编程是对行为进行抽象，数据与行为并存。
- 函数式编程使得编写`回调函数`和`事件处理程序`时，不必再纠缠与匿名内部类的弱可读性以及冗繁性。
- 增加了default方法，丰富了接口设计中的操作。
- 函数式编程核心: 使用不可变值和函数，函数将一个值进行处理，转变为另一个值。

## Lambda 表达式

Java8 最大的变化就是引入了lambda表达式，一种紧凑的、传递行为的方式。

### Lambda 表达式的几种表达方式

- `Runnable noArgmants = () -> System.out.println("Hello World");`
- `ActionListener oneArgument = event -> System.out.println("button clicked")`
- ``` Runnable multiStatement = () -> {
    System.out.println("Hello");
    System.out.println("World");
}```
- `BinaryOperator<Long> add = (x, y) -> x + y` // 参数类型由编译器推断出来
- `BinaryOperator<Long> add = (Long x, Long y) -> x + y` // 显示指定参数类型

### Lambda 表达式引用的是值，而不是变量

Lambda 表达式引用的是值，而不是变量，即该变量是既成事实上的final，`可以不声明为final，但是该变量只能被赋值一次`。

### 函数式接口 

函数式接口是指只含有一个抽象方法的接口，用作lambda表达式的类型，用来表示行为。为了表示该接口作为函数式接口来用，最好标识 `@FunctionalInterface`.

常用的jdk中提供的函数式接口：

- Predict<T> 参数T， 返回boolean
- Consumer<T> 参数T， 返回void
- Supplier<T> 参数 None， 返回T
- Function<T, R> 参数T，返回R
- UnaryOperator<T> 参数T， 返回T
- BinaryOperator<T, T> 参数T，T 返回T

### 类型推断

在java7中，就有了用菱形操作符来使得javac编译器自动推断类型， 例如

```
Map<String, String> map = new HashMap<>(); // 变量类型推断
Map<String, String> map1 = new HashMap<String, String>()
```
同样在java7中，将构造函数直接传递给一个方法，编译器可以通过方法签名来做推断，使得泛型可以被省略。
java8 更进一步，lambda表达式可以省略所有参数类型。

## Stream

## 类库

## 高级集合类和收集器

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
