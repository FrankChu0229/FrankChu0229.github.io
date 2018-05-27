title: Python Cookbook Notes Data Structure
date: 2018-05-27 20:39:51
tags: [python, coding, data structure]
categories: python
description: Python Cookbook Notes: Data Structure.
---

## 解压可迭代对象赋予多个值

```
a = [1,2,3,4,5,6]
x,*y,z = a

```
You will get `x=1, y=[2,3,4,5], z=6`.

## 保留最后n个元素

可以使用`collections.deque`双向队列来实现, `deque(maxlen=N)` 可以定义一个长度为N的队列。

`queue = deque(5)`,

- `queue.append()` 从队尾append
- `queue.pop()` 从队尾pop
- `queue.appendleft()` 从队首插入
- `queue.popledt()` 从队首pop

## 查找最大或者最小的N个元素

可以使用python中的堆, `import heapq`, heapq 中提供了`nlarest` 和`nsmallest` 方法.

堆是一种特殊的数据结构，是一个完全二叉树。根据parent节点值不大于(小于)其左右子节点值，堆又可以分为最小堆和最大堆。python 中的heapq 提供了堆的数据结构实现，以及相关的堆算法。

```
- heapq.nlargest(n, heap, key=None), key 为类似sorted中的key
- heapq.nsmallest(n, heap, key=None)
- heapq.heapify(list), to construct a heap given a list
- heapq.push(heap, item)
- heap.pop(heap, item)
```

---
