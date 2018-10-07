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


#### 惰性求值操作

- map 将一种类型的值转变为另一种值, 即接受一个Function<T, R>的函数式接口
    ```
     List<String> collected = Stream.of("a", "b", "c").map(String::toUpperCase()).collect(Collectors.toList());   
    ```
- filter 接收一个Predicate<T>, 即输入为T，返回boolean型变量
    ``` 
    int count = Artists.stream().filter(Artist::isFrom("London")).count();
    ```
- flatMap flattern Stream of stream and then map, map接收T类型参数，返回Stream，即Function<T, Stream>
    ```
    List<String> collected = Stream.of(asList("a", "b"), asList("c", "d")).flatMap(List::stream).collect(Collectors.toList());
    ```
- distinct 
- limit

#### 及早求值操作/终止型操作

- collect 

 ```
    List<String> collected = Stream.of("a", "b", "c") // Stream 的of方法使用一组初始值来生成新的Stream
    .collect(Collectoris.toList());
 ```
- max 传入Comparator T对象, 返回Optional<T>
    ```
      List<Track> tracks = Arrays.asList(new Track("a", 524), new Track("b", 378, new Track("c", 451)));
      Track maxTrack = tracks.stream().max(Comparator.comparing(Track::getLength())).get(); // java8 中，comparing作为工厂方法可以接收一个函数表达式，返回一个Comparator, Comparator有且仅有一个抽象接口，因此为函数式接口。
    ```
- min
- count
- reduce 更通用的方式, reduce 可以从一组值中生成一个值，像min，max，count等都属于reduce操作. 对于一组值的迭代，通常可以用以下方式来进行：
    
    ```
    Object accumulator = initValue;
    for (Object element : collections) {
      accumulator = combine(accumulator, element)
    }
    ```
在这个计算中，只有initValue和combine是不确定的，因此只需要指定这两个就可以实现一个reduce操作。即reduce操作接收一个初始值和一个BinaryOperator操作。

```
int count = Stream.of(1,2,3).reduce(0, (acc, element)-> acc + element );
```
以上例子用reduce实现了一个sum的操作。

- findAny, findFirst, anyMatch, allMatch, forEach
- collect

### 正确使用lambda表达式

lambda表达式的使用应该是`无副作用`的，即`只通过函数返回值就能充分理解函数的作用`。

- 在lambda表达式内部使用局部变量，该变量应该是`既成事实上必须是final的`
- forEach 方法是一个终结方法，可以有副作用。

## 类库

## 高级集合类和收集器

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
