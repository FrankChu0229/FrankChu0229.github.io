title: Design Pattern
date: 2018-02-27 17:21:54
tags: [engineering, design pattern]
categories: Design Pattern
description: Design pattern, examples implemented in java.
---

## Singleton Instance

```
public class Test {
	
	private static final class SingletonHolder {
		private static final Test INSTANCE = new Test();
	}
	
	private Test() {
	}
	
	public static Test getInstance() {
		return SingletonHolder.INSTSNCE;
	}
	
	public static void main(String[] args) {
		Test test = Test.getInstance();
	}

}

```
说明: 静态内部类在使用时才加载，所以静态内部类的写法可以做到延迟加载.同时又是线程安全的。



---
