title: Python Underline Usage
date: 2018-05-28 12:48:07
tags: [python, coding]
categories: python
description: Python 中下划线的使用总结。
---

## 用作变量名

`_`有时会被用作变量名，表示是一个临时变量，后面不会被用到。

```
for _ in range(5):
    print('hahaha')
```

## 放在变量名前

### `_var` or` _func`

在变量名前加一个`_`表示这个变量名或者方法是私有的，不对外开放。

### `__var` or `__func`

在变量名前加`__`双下划线，表示该变量或者方法不可被覆盖. 实际上python内部通过在该变量名或者方法名前添加`_classname__methodName`的方式来防止覆盖。

```
class A(object):
    def __init__(self):
        pass
    def __method_show(self):
        print("A")

class B(A):
    def __init__(self):
        pass
    def __method_show(self):
        print("B")

if __name__ == '__main__':
    a = A()
    print(dir(A))
    print(dir(B))
['_A__method_show', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
['_A__method_show', '_B__method_show', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']

```

### `__var__` 

被用作python中的特殊方法名，保证不会和用户自定义的方法名冲突，如`__name__`, `__init__`等。

## Reference 

- [reference](http://python.jobbole.com/81129/)
- [refercnce](http://www.cnblogs.com/coder2012/p/4423356.html)
