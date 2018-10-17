title: Java8 Functional Programming-4
date: 2018-10-15 15:41:32
tags: [coding, summary, java, functional programming]
categories:  Java 
description: Java8 Lambdas Functional Programming Summary.
---

## 高级集合类和收集器 (Collector)

Java8 除了对Collection类改进之外(增加stream等)，还增加了Collector(收集器)以及Collectors(Collector各种实现的工厂)；方法引用等。

### 方法引用

方法引用可以看作是对lambda表达式的重写，方便对已有方法的重用。方法引用的标准语法为`ClassName::methodName`, 这里方法后面没有参数，因为不是对方法进行调用。例如对lambda表达式
```
artist -> artist.getName()
```
等同于
```
Artist::getName
```


```
name -> new Artist(name)
```
等同于

```
Artist::new
```

### 元素顺序

Stream 中的元素以何种顺序进行排列，取决于`数据源`和`对流的操作`。

- 例如，数据源是List，则流中的出现顺序和list中元素顺序是一致的；如果数据源是`HashSet`，则流中的元素也是无序的。
- 对流的操作，也会对流中元素顺序产生影响

```
List<Integer> list = Stream.of(1,2,3,6,5).sorted().collect(Collectors.toList());
assertThat(list, hasItem(2)); // assertThat 
```

### 收集器 Collector && Collectors

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
