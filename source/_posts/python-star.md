title: Python中*的用法
date: 2018-05-28 10:18:47
tags: [python, coding]
description: Python 中*的用法。
---

## *args 与 **args

*args 常作为变量用在func中，表示一组(list or tuple)变量

```
def add(*args):
    sum = 0
    for num in args:
        sum += num
    return sum
if __name__ == '__main__':
    print(add(1,2,3,4,5))
```

```
def test(**kargs):
    for key, value in kargs.items():
        print(key,value)

if __name__ == '__main__':
    test(a=1, b=2)

```

## 在list或tuple赋值中使用*

```
iterData =  list(range(100))
begin, *middle, end = iterData
print(begin, middle, end)
```

## 在zip()中使用的*

```
def test_zip():
    a = [1,2,3,4,5]
    b = [6,7,8,9,10]
    c = list(zip(a,b)) # 解压
    print(c)
    print(list(zip(*c))) # 压缩
```

## Reference

- [python cookbook](http://python3-cookbook.readthedocs.io/zh_CN/latest/c01/p02_unpack_elements_from_iterables.html)
- [problems solving using algorithms and data structures](http://interactivepython.org/runestone/static/pythonds/index.html)

