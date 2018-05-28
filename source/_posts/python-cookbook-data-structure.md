title: Python Cookbook Notes Data Structure
date: 2018-05-27 20:39:51
tags: [python, coding, data structure]
categories: python
description: Python Cookbook Notes Data Structure.
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
heapq.nlargest(n, heap, key=None), key 为类似sorted中的key
heapq.nsmallest(n, heap, key=None)
heapq.heapify(list), to construct a heap given a list
heapq.heappush(heap, item)
heap.heappop(heap, item)
```

### 时间复杂度

- 堆的插入和删除复杂度 O(logn), 查询O(n).
- 对长度为N的list，选出n个最小的元素，复杂度为O(Nlogn), 对N个元素进行遍历，维护一个大小为n的最小堆，每次插入复杂度为O(logn).

## 实现一个PriorityQueue

实现一个PriorityQueue， 并且在pop的时候，优先级最高的元素先出。可以考虑用heap，python中的heapq是一最小堆， 因此需要将priority取负数，同时添加index，使得当priority相同的时候可以通过index来进行比较。

```
import heapq

class PriorityQueue:
    def __init__(self):
        self._queue = []
        self._index = 0
        
    def push(self, item, priority):
        heapq.heappush(self._queue, (-priority, self._index, item))
        self._index += 1
    
    def pop(self):
        return heapq.heappop(self._queue)[-1]
        
class Item:
    def __init__(self, name):
        self.name = name

```
这里需要提到的是，如果直接比较`Item('a') < Item('b')`, 是不支持的(需要override __gt__等方法)， 但是`(1, Item('a')) < (2, Item('b')` 元组之间是可以进行比较的。

---
