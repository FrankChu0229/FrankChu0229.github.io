title: Java中的String, StringBuilder和 StringBuffer
date: 2018-02-26 23:05:41
tags: [java, notes, engineering]
categories: java
description: Java 中的String, StringBuilder和 StringBuffer
---

## StringBuilder 与 StringBuffer

- StringBuilder 执行速度比StringBuffer要快，但是StringBuilder不是线程安全的
- StringBuffer 是线程安全的，执行速度比StringBuilder要慢


## String

在java中，String字符串常量是不可改变的对象，每当在String上进行操作时，实际上都是在创建一个新的String对象。以下代码：

```
1. String s = "abx";
2. String s = s + "ds";

```
其中，第二行中的`s`会被重新创建，原来的对象会被GC掉。而StringBuilder和StringBuffer是可变对象，在同一个对象上进行操作。

值得注意的一点是：

```
String s = "this " + "is " + "my " + "home"

```
等同于`String s = "This is my home"`, 在这种情况下String操作要比StringBuilder和StringBuffer要快，是JVM的一个把戏。但是当字符串拼接中有其他的String变量时，JVM就会按照原来的方式来做。

## Reference

- http://www.cnblogs.com/A_ming/archive/2010/04/13/1711395.html

---