title: Java8 Functional Programming-2
date: 2018-10-07 11:05:47
tags: [coding, summary, java, functional programming]
categories:  Java 
description: Java8 Lambdas Functional Programming Summary.
---

## Stream

Java8 对核心库的更改主要包括对集合类新增API和引入Stream，Stream使得程序员能够在更高的抽象层级上对集合进行操作。

`Stream 是函数式编程在集合类上进行复杂操作的工具`

先来看一个例子，在java8之前，我们通常会写很多这样的代码:

```
int count = 0;
for (Artist artist : Artists) {
  if (artist.isFrom("London")) {
    ++count;
  }
}
```

这种通过for循环的写法，实际上是封装了迭代的语法糖，它属于`外部迭代`(通过Iterator对象的hasNext和next方法来完成迭代)，等价于

```
int count = 0;
Iterator<Artist> iterator = Artists.iterator();
while (iterator.hasNext()) {
  Artist artist = iterator.next();
  if (artist.isFrom("London")) {
    ++count;
  }
}
```

然而通过外部迭代的方式有以下若干缺陷：

- 串行化操作，for循环变成并行方式比较麻烦
- 方法和行为混在一起

我们可以通过`内部迭代`的方式来实现，即

```
int count = Artists.stream().filter(Artist::isFrom("London")).count();

```

这种实现可读性也更好一些。

## 类库

## 高级集合类和收集器

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
