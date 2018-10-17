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

Stream操作中的collect(及早操作)接收Collector(收集器)，在Collectors中，有一些常用的Collector实现：

- 转换为其他集合，`Collectors.toList()`, `Collectors.toSet()`；如果想指定Collection类型，可以使用`Collectors.toCollection(TreeSet::new)`
- 转换为值，`maxBy(comparing(Artist::getCount))` 返回数量最大的Artist；`averagingInt(Artist::getCount)`, `summingInt(Artist::getCount)`进行相关数值计算，和IntStream等中的操作类似
- 数据分块，`partitioningBy(Predicate)`, 接收一个Predicate操作，将流分成两半，返回Map<Boolean, List<Artist>>, 即为true和false两种情况
- 数据分组，`groupingBy(Album::getMusican)`, 按照Musician对Album进行分组，返回Map<Artist, List<Album>>
- 字符串， `Collectors.joining(",", "[", "]")` to join elements in stream with spliter ",", begin "[" and end "]".
- 组合收集器 如果我们只想按照艺术家进行分组，但是想返回艺术家的专辑数量，groupingBy支持`下游收集器`，即`groupingBy(Album::getMusican, counting())`, 下游收集器会对分组后每块的元素进行相关的操作；同理还可以通过`groupingBy(Album::getMusician, mapping(Album::getName, toList()))` (mapping作为groupingBy的下游收集器， toList()作为mapping的下游收集器)按照艺术家进行分组，同时可以得到和每个艺术家相关的专辑名列表。

### 一些细节

Map类在引入lambda表达式之后，也做了相应改变，我们可以通过

- `computeIfAbsent(key, Function)` // 如果不存在, 可以通过Function函数式接口返回其他值
- `computeIfPresent(key, BiFunction)` // 如果存在，可以做相关的BiFunction操作来对map中的值进行修改
- `compute(key, BiFunction)`  // 不判断该key是否存在，可以通过Bifunction对map中的值进行修改等,可以看作是前两者的综合

我们在对map进行操作的时候，经常会实现这样的逻辑：


```
Map<String, Integer> map = new HashMap<String, Integer>();

if (map.contains(key)) {
    String value = map.get(key);
    Integer newValue = value + 1;
    map.put(key, newValue)
} else {
    map.put(key, 1);   
}

```

现在我们可以这样实现：

```
Map<String, Integer> map = new HashMap<String, Integer>();

map.computeIfPresent(key, (key, value) -> value + 1);
map.computeIfAbsent(key, key -> 1); // OR map.putIfAbsent(key, 1);

```

或者可以通过compute

```
Map<String, Integer> map = new HashMap(String, Integer);
map.compute(key, (key, value) -> (value == null) ? 1 : value + 1 );
```

## 数据并行化

## 测试、调试和重构

## 设计和架构的原则

## 使用Lambda表达式编写并发程序

## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
- [java8 Map compute](http://blog.tanpeng.net/2017/07/13/map-compute/)


