title: Java8 Functional Programming-3
date: 2018-10-07 11:05:47
tags: [coding, summary, java, functional programming]
categories:  Java 
description: Java8 Lambdas Functional Programming Summary.
---

## 类库

Java8 中另一个变化是引入了接口的默认方法和接口的静态方法，使得接口中也可以包含代码体了。

### 对基本类型进行优化的Stream

在java中，类型可以分为基本类型和装箱类型，例如，int为基本类型，Integer为装箱类型。基本类型内建在语言和运行环境中，是程序构建的基本模块；装箱类型属于普通的java类，为对基本类型的一种封装。Java中范型可以看作是对范型参数类型的擦除，即假设他是Object的实例，因此只有装箱类型才能作为范型参数。这就解释了为什么声明一个`List<int>`的列表，得到的却是`List<Integer>`的列表，即包含了整型对象的列表。 将基本类型转换为装箱类型为`装箱`操作，将装箱类型转换为基本类型为`拆箱`操作。

装箱类型是普通的java class对象，装箱类型在存储和计算上有着相关的缺陷：

- 在存储上，由于装箱类型是对象，因此相比基本类型存在更大的内存开销 (指向对象的指针等存储开销)
- 基本类型和装箱类型之间的装箱和拆箱操作会引入额外的计算开销

对于会涉及到大量数值运算的算法来说，装箱、拆箱操作以及装箱类型引入的额外内存存储，都会对程序运行性能造成一定的影响。

因此，Stream中的某些方法对基本类型和装箱类型做了区分。在java8中只对`int`, `long` 和`double`类型做了特殊处理，因为它们在数值运算中用到的比较多，经过特殊处理之后，系统性能提升明显。

#### Best Practice: 用Stream做数值运算

- 如果Collection包含的已经为`int`, `long`, `double`基础类型数据，那么创建`IntStream`,`LongStream`, `DoubleStream` ASAP
- 如果Collection为其他类型，那么在做完filter等操作之后，尽快使用`mapToInt`, `mapToLong`, `mapToDouble`, 将stream转换为相应的`IntStream`,`LongStream`, `DoubleStream`. 

#### 几种命名Convention

- `ToLongFunction`, R -> long, 如果为int、long、double，则可以使用相应的`LongToIntFunction`, `LongToDoubleFunction`
- `mapToLong` 接收`ToLongFunction`, 相应的，在IntStream中, `mapToLong`方法接收`IntToLongFunction`函数式接口。


## 高级集合类和收集器

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
