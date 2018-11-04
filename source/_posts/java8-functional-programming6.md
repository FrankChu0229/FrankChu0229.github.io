title: Java8 Functional Programming-6
date: 2018-10-28 22:23:34
tags: [coding, summary, java, functional programming]
categories:  Java 
description: Java8 Lambdas Functional Programming Summary.
---

## 测试、调试和重构

本部分侧重于lambda表达式的测试，调试以及将已有代码重构为lambda表达式的形式。

### 利用lambda表达式进行重构

在之前的例子中，我们主要使用lambda表达式对集合操作进行重构，这里我们更深入一些，看看什么时候适合将我们的代码lambda化。

#### 进行行为的传递

和面向对象编程相比，lambda表达式是对行为的抽象，可以进行行为的传递；有很多模式多类似，但是行为有所不同，面向对象编程是对数据的抽象，不是很好能解决这个问题，例如，

在一个Order类中，我们想对用户购买的专辑做一些统计, 命令式的实现如下


```
public long countRunningTime() {
    long count = 0;
    for (Album album : this.albums) {
    for (Track track : album.getTrackList()) {
            count += album.getLength();
        }
    }
    return count;

}

public long countMusicians() {
    long count = 0;
    for (Album album : this.albums) {
         count += album.getMusicianList().size();
    }
    return count;

}

public long countTracks() {
    long count = 0;
    for (Album album : this.albums) {
        count += album.getTrackList().size();
    }
    return count;

}

```

我们可以使用集合的stream进行一定的重构：

```
public long countRunningTime() {
    return this.albums.stream().mapToLong(album -> album.getTrackList().stream().mapToLong(track -> track.getLength()).sum()).sum();
}

public long countMusicians() {
    return this.albums.stream().mapToLong(album -> album.getMusicianList().size()).sum();
}

public long countTracks() {
    return this.albums.stream().mapToLong(album -> album.getTrackList().size()).sum();
}
```

进而，我们可以将行为进行抽象，使用`ToLongFunction`函数式接口，

```

public long countFeature(ToLongFunction function) {
    return this.albums.stream().mapToLong(function).sum();
}

public long countRunningTime() {
    return countFeature(album -> album.getTrackList().stream().mapToLong(track -> track.getLength()).sum());
}

public long countMusicians() {
    return countFeature(album -> album.getMusicianList().size());
}

public long countTracks() {
    return countFeature(album -> album.getTrackList().size());
}

```

#### 使用lambda表达式代替匿名内部类

和内部匿名类相比，lambda表达式可读性更好，代码实现更简洁。例如，

```
ThreadLocal<Album> thisAlbum = new ThreadLocal<Album>() {
    @Override protected Album initValue() {
        return database.lookupCurrentAlbum();
    }
}


ThreadLocal<Album> thisAlbum = ThreadLocal.withInitial(() -> database.lookupCurrentAlbum());
```

#### 使用java8内置类库中的新接口

在[这里](http://frankchu.tech/2018/10/15/java8-functional-programming4/)我们提到了用java8中的`computeIfPresent()`, `computeIfAbsent()`, `compute()`, `putIfAbsent()`等简化对map的操作。

## 设计和架构的原则



## 使用Lambda表达式编写并发程序



## Reference
- [Java 8 Lambdas: Functional Programming for the Masses](https://www.amazon.com/Java-Lambdas-Functional-Programming-Masses/dp/1449370772)
    

