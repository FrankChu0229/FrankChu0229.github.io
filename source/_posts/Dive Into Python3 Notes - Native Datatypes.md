title: Dive Into Python3 Notes Native Datatypes
date: 2017-10-19 16:15:23
tags: [Python, notes, summary]
categories: [python, notes]
description: Dive Into Python3 Notes - Native Datatypes
---

# Dive Into Python3 Notes - Native Datatypes

在python中，任何value都有type，但是你不需要显式地声明value的类型。Python根据该变量第一次被赋值的不同来对该变量的类型做出推断。

Python中主要的数据类型有：

- Boolean： `True` or `False`
- Numbers： they can be integers，floats，fractions(1/3) and even complex numbers
- Strings: an sequence of Unicode characters
- Bytes and byte arrays: e.g., an jpeg image
- List: ordered sequence of values
- Set: **unordered** sequence of values
- Tuple: ordered and **immutable** sequence of values
- Dict: unordered key-value pairs

## Number

- use `type()` and `isinstance()` to get the datatype and judge
- use `int(2.0)` and `float(1)`, `int(-2.5) = -2` 来做type转换
- float numbers 小数点后最多只能有15位
- 与python2既有int又有long不同， python3中只有int， 并且int可以取得像python2中long的最大值

### Number Operations

- `/` 除法， 返回float类型
- `//` 整数除法： `11 // 2 = 5`, `-11 // 2 = -6` 
- `**` 幂操作
- `%` 取余数

### 分数 Fractions

- 使用 `import fractions`, `print(fractions.Fraction(1, 3))`

### 其他数字

- PI : `import math`, `print(math.pi)`
- 需要注意的是python中没有infinite的精度， 因此像`sin(\pi)`的结果不是0， 而是一个很小的数

### 数字用在 bolean context

- 如果数字的value为0， 则为False； 否则为True


## List

Python中list大小可以动态改变，并且list中可以add各种不同类型的value。

### Define a list

Define a list `a = []` or `a = [1, 'hello', 1.0, [1,2,3]]`

### Get the values from the list

Get the values from the list: `a[0], a[1], a[2], a[3]` or `a[-1], a[-2], a[-3], a[-4]`

### Slicing the list

Slicing the list: `a[1:3]` can get the sublist of `a[0]` and `a[1]`, 即不包括最后的一位；如果前后有省略，则默认为从该list的第一位开始(包含)，或者到最后一位(包含)；如果前后都没有，则结果为整个list

```
a[:2] -> [1, 'hello']
a[2:] -> [1.0, [1,2,3]]

```
### Adding items to list: 

Four ways to add items to a list:

- Operation `+` : `a = [1, 2] `, `b = [2] `, `a = a + b -> [1,2,2]`, 这里需要注意的是， `+`操作会new 一个新的list
- `a.append()`函数, `append()`函数会把append的内容(无论什么类型)加到list中, 例如： `a.append([4,5,6]) -> a = [1,2,2,[4,5,6]]`
- `a.extend([4,5,6])`函数会把extend的内容一个一个的加到list中， 得到`a = [1,2,2,4,5,6]`
- `a.insert(0, '4')`, 会把value插入到指定的index处


### Searching in a list

- `a_list.count('a')`: return the number of specific values in a list
- `'a' in a_list`, return a boolean value to show if the value is in the list
- `a_list.index('a')` to return the index of the given value

### Removing items in a list

- `del a_list[1]` to delete the value in the index of 1
- `a_list.remove('a')` to remove the first occurance of the value in a list
- `a_list.pop()` removes the last value in a list
- `a_list.pop(0)` removes the value in the given index

### List in a boolean context

- empty list will be `False` in a boolean context and list which is not empty will be `True`.


## Tuples

A tuple is an immutable list. A tuple cannot be changed once it is created.

Tuple和List比较像， tuple可以看做是一个不可变的list。Tuple比list更快，如果这个set是不会被更改的，则使用Tuple，同时它也会使你的code更加安全。

在Python中，只有immutable的才可以作为Dict 的key，如果tuple中的变量都是immutable的，那么

### Tuple的创建和获取元素

- `a_tuple = (1,2,3,Ture)`, Tuple用圆括号来创建，元素获取的方式同list
- Tuple的slice方式`a_list[1:3]`同list，不同之处在于，tuple的slice返回的仍是tuple

### Tuple中元素的操作

- Tuple元素的操作不包括：如 `pop()`, `append()`等添加或者删除元素的函数
- Tuple中可以使用`count()`和`index()`, `'a' in a_tuple`等操作 

### Tuple 和 List的转换

- `tuple()` functuon can convert list into tuples and `list()` function can convert tuples into list. 

### Tuples in boolean context

- 同list一样， 在boolean context中，如果tuple为空，则返回False； 否则返回True
- 值得注意的是，在创建只有一个值的tuple时，应为`(False,)`；即需要加一个逗号”，“， 否则python无法识别出是带有一个值的tuple。

### 将tuple用来同时给多个变量赋值

- `(x, y, z) = (1,2,3)`


## Set

在python中， set是无序的，并且没有冗余元素的数据结构。A single set can contain values of any immutable datatype, 因此像list不能是set中的元素，因为list不是immutable的，但是tuple可以。

### Set的创建

- `a_set = {1,2,3}` 可以用来创建一个set， 但是`{}`为一个空的dict； `set()`才是创建一个空set的方式。
- 同时也可以使用`set([1,2,3])`函数来将一个list转换为一个set，此操作不会对list做改变。

### 增加和删除set中的元素

#### 增加set中的元素： 

- `a_set.add({1,2,3})`, 同list中的`append()`一样， 会将传进去的datatype加到一个变量中去
- `a_set.update({1,2,3}，{1，2，3})`, 同list中的`extend()`一样， 会将set或者list中的每一个元素加到set中，并且会去除重复元素，保证unique。


#### 删除set中的元素：

- `a_set.discard(1)` 将会删除set中的变量1，如果变量1不存在，没有任何影响；
- `a_set.remove(1)`将会删除set中的变量1，如果变量1不存在，则会raise Exception KeyError。
- `a_set.pop()` 将会随机删除set中的一个元素，因为set是无序的，因此不能像list一样pop掉最后的元素。
- `a_set.clear()` 将会clear掉set的全部元素，返回一个空的set

### Set的操作

- `1 in a_set` will return True if a_set contains 1, otherwise return False
- `a_set.union(b_set)` 将会把 a\_set和b\_set取并集
- `a_set.intersection(b_set)` 将会取a\_set 和 b\_set 的交集
- `a_set.difference(b_set)` 将会取在a\_set 但是不在b\_set中的元素
- `a_set.symmetric_difference(b_set)` 将会取只出现在a\_set 和 只出现在b\_set的并集

### Set in a boolean context

- 同list 和tuple 一样，空的set在boolean context中为False，否则为True


## Dictionary

在python中，Dictionary为无序的key-value pair的set。当向dict中添加一个key的时候，你也必须添加一个相应的值。

### Dict的创建和元素获取

- `{}` 为空的dict， `a_dict = {'1': 'h', '2': 'w'}` 创建一个dict
- `a_dict['1']` 根据相应的key，来获取对应的value； 当`a_dict['3']` 获取一个未包含的key的时候，会raise KeyError Exception

### Dict的更改

- `a_dict['3'] = 'hhh'`来增加新的key-value pair
- `a_dict['1'] = 'hh'` 来对已经存在的key-value pair进行更改

### Mixed Value Dict

Dict 的value可以是任何类型， 但是dict的key 需要时immutable的， 例如： integer， string， tuple等。

- 和list、set、tuple等一样, dict可以用`len()`来返回key-value pair的个数
- 和list、set、tuple等一样， 可以使用`in` 来判断一个变量是不是dict中的一个key

### Dict in a boolean context

- An empty dict is False
- Otherwise, it's True

## None

- None 是Python中一个比较特殊的constants，它是一个null值。
- None有他自己特殊的type，为NoneType
- 将None同其他不是None的值进行比较，都将返回False。
- 所有的None值 (值为None的变量) 都相等。

## Reference

- [Dive into Python 3] (http://www.diveintopython3.net/)

---