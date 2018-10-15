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

### 重载解析

在java中，相同方法名，不同参数类型的方法可以被重载。但重载可能会对类型推断造成影响，因为可能会有多种类型被推断出来，javac会选择`最具体的`那种类型作为推断结果；然而有的时候推断`最具体的`类型可能会有问题，这时javac会报错。例子如下：

```
public class Demo {

  interface TestPredicate extends Predicate {

  }

  public void overLoad(Predicate predicate) {
    System.out.println("Predicate");
  }

  public void overLoad(TestPredicate predicate) {
    System.out.println("TestPredicate");
  }

  public void test(Object object) {
    System.out.println("Object");
  }

  public void test(String string) {
    System.out.println("string");
  }

  public static void main(String[] args) {
    Demo demo = new Demo();
    demo.test("ss");
    demo.overLoad(x -> false);
  }
}

```
但是当TestPredicate 不是Predicate的子类时，javac会找到多个类型推断的结果，进而报错：

```
public class Demo {

  interface TestPredicate<T> {
    boolean test(T t);
  }

  public void overLoad(Predicate predicate) {
    System.out.println("Predicate");
  }

  public void overLoad(TestPredicate predicate) {
    System.out.println("TestPredicate");
  }

  public void test(Object object) {
    System.out.println("Object");
  }

  public void test(String string) {
    System.out.println("string");
  }

  public static void main(String[] args) {
    Demo demo = new Demo();
    demo.test("ss");
    demo.overLoad(x -> false); // ambiguous method call. 
  }
}
```

因此，在方法重载和类型推断中，
- 如果只有一种方法，则javac会推断出相关类型
- 如果有多个方法，则javac会选出`最具体的`那种类型
- 如果推断出多个类型，javac会报错，此时需要人为制定类型。

### @FunctionalInterface

在定义函数式接口的时候，最好在前面加上`@FunctionalInterface`注解，这样方便使用方清楚它是一个函数式接口，在重构的时候，如果改成多个方法，javac就会报错。值得一提的是，在java中只有一个方法的接口，并不一定是想使用lambda表达式来实现的，比如`Closable`和`Comoparable`接口都只有一个方法，这种属于纯属巧合。

### 默认方法

二进制兼容是java的一大特性，Java8中最大的修改是在Collection接口增加了stream等方法，这意味者所有实现了Collection的接口都要实现stream()等的方法。java核心库可以做相关的实现，但是在核心库之外的实现了Collection接口的类就不能二进制兼容了，为了解决这个问题，java8中引入了`default`接口。 

相应的在Java8中，Iterable中也增加了foreach的default接口。

**接口是一种约定方式，接口中的成员变量都是静态的，即默认修饰符public static final的；成员方法都是抽象的，即默认此时符 public abstract**



## 高级集合类和收集器

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
