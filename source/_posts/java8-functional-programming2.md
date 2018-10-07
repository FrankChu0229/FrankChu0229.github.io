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

### 实现机制

在上面的操作中，我们分为过滤和计数两个步骤来进行实现，那么上面基于Stream的方式是怎么实现的呢，是不是比for循环的方式复杂度要高呢？

在Stream中，操作主要分为：`惰性求值`和`及早求值`两种方式。惰性求值返回值仍是stream，而及早求值返回另一个值或者null。Stream中的多个惰性求值操作形成一个惰性求值链，最后由一个及早求值操作返回想要的结果。这也意味着可以有多个级连操作，但是只迭代一次。

例如，在上面的的例子中，filter就是惰性求值操作，count是及早求值操作。

### 常见Stream操作


## 类库

## 高级集合类和收集器

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
