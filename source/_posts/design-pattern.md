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




---