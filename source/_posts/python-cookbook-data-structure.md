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
- `queue.popleft()` 从队首pop

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
这里需要提到的是，如果直接比较`Item('a') < Item('b')`, 是不支持的(需要override \_\_gt\_\_等方法)， 但是`(1, Item('a')) < (2, Item('b')` 元组之间是可以进行比较的。

## DefaultDict 和 OrderedDict

- `defaultdict` 可以为每一个key的值进行初始化，常用在一个key有多个值的情况，即该key对应了list()或者tuple.
- `OrderedDict` 在构建dict的时候，可以保存将pair put进dict中的顺序，类似于java中的`LinkedHashMap`. Python 中的OrderedDict维护了一个根据键值插入顺序的双向链表， 即每次该键值插入时，会在链表尾部添加该键值, 重复的键值不会影响链表机构。因此， OrderedDict 的内存开销是正常dict开销的两倍，在内存消耗较大的场景需要谨慎考虑是否使用OrderedDict.

```
from collections import OrderedDict
from collections import defaultdict
d = defaultdict(list)
d['a'].append(1)
d['a'].append(2)
print(d['a'])

orderedDict = OrderedDict()
orderedDict['a'] = 'a'
orderedDict['b'] = 'b'

print(orderedDict)

```
## 词典的运算

- 在需要对词典的值进行排序，取最大最小值的同时，返回该值的键值，可以用下面的方法快速实现：

```
data = dict()
data['a'] = 1
data['b'] = 2
data['c'] = 10
print(data)
print(min(zip(data.values(), data.keys()))) # zip()为只可迭代一次的迭代器
print(max(zip(data.values(), data.keys())))
print(sorted(zip(data.values(), data.keys())))
```

- 在需要查找两个词典的相同处和不同处的时候，可以使用：

```
a = {'a':1, 'b':2, 'c':3}
b = {'a':1, 'd':5, 'z':6}
print(a.keys() & b.keys())
print(a.keys() - b.keys())
print(a.items() & b.items())
print(a.items() - b.items())

```
- dict的items()和keys() 返回一个集合，因此可以使用集合的操作来实现。但是dict的values()不能使用这个功能，主要是values()的值不能保证完全unique，可以先将values()转换成set，然后在操作.

## Counter

```
from collections import Counter
a = [1,2,3,4,5,1,2,4,5,6,7]
b = ['a', 'b', 'c']
counter = Counter(a)
print(counter.most_common(3)) # get top 3 
print(counter[1]) # 内部本质就是dict
print(counter[2])
a + b # Counter支持 集合操作
a - b 
```

## `itemgetter()` && `attrgetter()`

在python中，`sorted(iterable, cmp=None, key=None, reverse=False)`、`min()`等函数常有key可以自定义一些行为。当然这里我们可以传入一个function对象，或者用一个lambda表达式来解决。但是其实我们可以使用`operator`中的`itemgetter()`和`attrgetter()`来提高效率。

```
rows = [
    {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
    {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
    {'fname': 'John', 'lname': 'Cleese', 'uid': 1001},
    {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
]
from operator import itemgetter
a = sorted(rows, key=itemgetter('uid'), reverse=True) # itemgetter() 常用在支持原生比较的对象
a1 = sorted(rows, key=lambda r: r['uid'])
b = sorted(rows, key=itemgetter('fname'))
b1 = sorted(rows, key=lambda r: r['fname'])
print(a)
print(a1)
print(b)
print(b1)

```

```
from operator import attrgetter
class User:
    def __init__(self, user_id):
        self.user_id = user_id
    
    def __repr__(self):
        return "User({})".format(self.user_id)

users = [User(1), User(5), User(3)] 

a = sorted(users, key=lambda u: u.user_id, reverse=True)
b = sorted(users, key=attrgetter('user_id')) ## attrgetter() 常用在不支持原生比较的对象
print(a)
print(b)

```

## `itertools.groupby()` 通过某个字段进行分组

```
rows = [
    {'address': '5412 N CLARK', 'date': '07/01/2012'},
    {'address': '5148 N CLARK', 'date': '07/04/2012'},
    {'address': '5800 E 58TH', 'date': '07/02/2012'},
    {'address': '2122 N CLARK', 'date': '07/03/2012'},
    {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
    {'address': '1060 W ADDISON', 'date': '07/02/2012'},
    {'address': '4801 N BROADWAY', 'date': '07/01/2012'},
    {'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
]

from itertools import groupby
from operator import itemgetter

sorted_rows = sorted(rows, key=itemgetter('date')) ## groupby() 只查找连续相同值，因此需要先根据该字段进行排序

for date, items in groupby(sorted_rows, key=itemgetter('date')): ## groupby 返回group的字段值，并且在该字段值下的一个迭代器对象
    print(date)
    for i in items:
        print('', i)

```

## `filter` 过滤序列中的元素

在python中我们像过滤一个序列中的元素，常用到推到式，例如:

```
nums = [n for n in range(10) if n > 5]
print(nums)
nums = [n if n > 5 else 0 for n in range(10) ]
print(nums)

```

但是在输入非常大的时候，由于推到式会将全部结果load进内存，导致内存消耗过大，这个时候我们可以使用生成器表达式来解决：

```
def print_nums(nums):
    for n in nums: 
        print(n)
        
nums = (n for n in range(100) if n > 3)
print_nums(nums)

nums = (n if n > 3 else 0 for n in range(100))
print_nums(nums)

```

有时会出现过滤条件比较复杂的情况，不能在推到式或者生成器表达式中简单的表达出来，这个时候可以使用python built-in function `filter`.

```
values = ['1', '2', '-3', '-', '4', 'N/A', '5']

def is_int(val):
    try:
        a = int(val)
        return True
    except:
        return False
    
filtered = list(filter(is_int, values)) ## filter 返回一个迭代器
print(filtered)

```

## 从字典中提取子集

从字典中提取子集有以下若干种方式，经试验，字典推倒式更清晰，并且性能最好

```
prices = {
    'ACME': 45.23,
    'AAPL': 612.78,
    'IBM': 205.55,
    'HPQ': 37.20,
    'FB': 10.75
}
tech_names = {'AAPL', 'IBM', 'HPQ', 'MSFT'}

p1 = {key:value for key, value in prices.items() if key in tech_names} ## 字典推倒式，更清晰，性能更好
print(p1)

p2 = dict((key,value) for key,value in prices.items() if key in tech_names)
print(p2)

p3 = {key:prices[key] for key in prices.keys() if key in tech_names}
print(p3)

```

## `namedtuple`


```
from collections import namedtuple

Stock = namedtuple('Stock', ['name', 'shares', 'price']) ## namedtuple 是标准元组(tuple)子类的一个工厂方法
s = Stock('alibaba', 8080, 25)
name, shares, price = s
print(name)
print(shares)
print(price)

def compute_cost(records):
    total = 0.0
    for rec in records:
        total += rec.shares * rec.price
    return total

records = [Stock('alibaba', 8080, 25), Stock('tencent', 8888, 20)]
print(compute_cost(records))

```
## 使用生成器表达式--转换并同时计算数据

在很多时候，我们会用`any`, `sum`等函数，但是如果我们先计算得到一个临时list， 再通过`any`等函数计算，会多一个步骤。更优雅的方式是使用生成器表达式来转换并同时计算数据。

```
nums = [1, 2, 3, 4, 5]
sum_value = sum(num*num for num in nums)
print(sum_value)

```

## `ChainMap`

现在假设你必须在两个字典中执行查找操作（比如先从 a 中找，如果找不到再在 b 中找）。 一个非常简单的解决方案就是使用 collections 模块中的 ChainMap 类。

一个 ChainMap 接受多个字典并将它们在逻辑上变为一个字典。 然后，这些字典并不是真的合并在一起了， ChainMap 类只是在内部创建了一个容纳这些字典的列表 并重新定义了一些常见的字典操作来遍历这个列表。

```
from collections import ChainMap

a = {'x': 1, 'z': 3 }
b = {'y': 2, 'z': 4 }

chain_map = ChainMap(a, b)
print(chain_map['x'])
print(chain_map['y'])
print(chain_map['z']) # 都有的话使用第一个dict中的元素，同理删除和更新也是更新第一个字典中的该字段的值

```



## Reference

- [Python Cookbook](http://python3-cookbook.readthedocs.io/zh_CN/latest/c01/p11_naming_slice.html)
- [reference for itemgetter()](https://www.cnblogs.com/100thMountain/p/4719503.html)
- [reference for attrgetter()](https://blog.csdn.net/qq_24861509/article/details/47690819)

---
