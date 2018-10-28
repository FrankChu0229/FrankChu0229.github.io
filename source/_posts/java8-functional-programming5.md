title: Java8 Functional Programming-5
date: 2018-10-17 23:17:37
tags: [coding, summary, java, functional programming]
categories:  Java 
description: Java8 Lambdas Functional Programming Summary.
---

## 数据并行化

### 并行和并发

并发是指具有多任务处理的能力，多任务共享时间段，但不一定是同时；并行是指多个任务在同时进行处理；两者的区别主要还是是否为`同时`。知乎中很好的一个回答

**你吃饭吃到一半，电话来了，你一直到吃完了以后才去接，这就说明你不支持并发也不支持并行。你吃饭吃到一半，电话来了，你停了下来接了电话，接完后继续吃饭，这说明你支持并发。你吃饭吃到一半，电话来了，你一边打电话一边吃饭，这说明你支持并行。并发的关键是你有处理多个任务的能力，不一定要同时。并行的关键是你有同时处理多个任务的能力。所以我认为它们最关键的点就是：是否是『同时』。**

现在，计算机处理器主频频率增长速度已经很缓慢了，现代处理器主要为多核处理器，例如带有32核，64核等处理器的服务器机器已经比较常见了。这种处理器的变化影响了我们软件设计的方式，如何利用好多核处理器来加速我们的程序设计也成为了越来越多人关心的话题。

### 流的并行化操作

如果已经是一个stream了，我们可以通过`parallel()`方法来拥有并行化的能力，如果是一个Collection，我们可以通过`parallelStream()`的方式拥有并行化的能力。

例如，之前我们用stream对数据操作的代码

```

return albums.stream().mapToInt(Album::getLength).sum()
```

现在可以通过parallelStream获得并行化能力

```

return albums.parallenStream().mapToInt(Album::getLength).sum()
```

从上面的代码中，我们可以很轻松的将串行的stream变为parallelStream，即可拥有并行处理的能力，但是前提是代码写的需要符合规定，即需要遵守一些规则，否则并行化处理可能会和预想中的结果不一致。

- reduce的操作，初始值需要为`组合函数的恒等值`，例如用reduce求知时，(acc, element) -> acc + element 则初始值必须为0； 相应的若为乘法，则初始值为1
- reduce操作的另一个限制是`组合操作`必须符合`结合律`, 意味着只要序列的值不变，组合操作的顺序不重要。
- 要避免`持有锁`, stream内部会做相应的同步操作，没有必要自己加锁

### 并行化性能

并行化操作parallelStream在数据量小的时候不一定会比串行化操作stream的速度要好，在数据量大的时候，并行化处理速度会好很多。

影响并行化操作的性能主要由以下几方面：

- `数据大小`
- `数据源结构`, 支持随机读取的数据结构如ArrayList，数组，IntStream.range等可以被任意分解，在并行操作中效果最好；像hashset，treeset这些数据结构，不易被公平分解，但是大多数是可被分解的，性能一般；像LinkedList对半分解很难，还有像Streams.iterate, BufferReader.lines长度未知，因此很难预测在哪里分解，有时可能要O(N)的复杂度才能将他们对半分解，性能较差。
- `装箱`， 基本类型比装箱类型速度要快
- `cpu核的数量`
- `单元处理开销`， 在底层，并行流使用了fork/join框架，递归式的分解问题，然后每段并行执行，最终由join合并结果，返回最后的值。因此并行时间主要消耗在分解合并与并行执行上，只有并行执行的时间远大于分解合并的时间，并行操作性能提升才会越明显。
- `有无状态`, 像`map`, `filter`, `flatMap`等操作属于无状态的，`sort`, `distinct`, `limit`是有状态的，无状态会比有状态获得更好的获得并行性能。 


### 并行化数组操作

Java8中也引入了一些对数组的并行操作, 这次操作都在`Arrays`中

- `parallelPrefix` 任意给定一个函数，计算数组的和(可以为任意的BinaryOperator)
    ```
    double sums = new double[100];
    Arrays.parallelSetAll(sums, i -> i);
    Arrays.parallelPrefix(sums, Double::sum) // 得到[0,1,3,6] ... 即[0,1,2,3]的相加。
    ```
- `parallelSetAll` 使用lambda表达式更新数组元素 
    ```
    double values = new double[100]
    Arrays.parallelSetAll(values, i -> i) // 为每个数组元素的值设定为它的下标的值，i为下标
    ```
- `parallelSort` 并行化对数组排序


## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
- [Answers From Zhihu](https://www.zhihu.com/question/33515481)
- [Fork/Join](http://www.infoq.com/cn/articles/fork-join-introduction)
- [Fork/Join](http://www.importnew.com/27334.html)
