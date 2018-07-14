title: Python Cookbook Number Summary
date: 2018-07-14 16:59:10
tags: [python, coding, notes]
categories: python
description: Python Cookbook Digit Summary.
---



## 数字的四舍五入 round

```
print(round(1.23, 1)) ## ndigits, 保留小数的位数
print(round(-1.27, 1))
print(round(1.55, 1))
print(round(1.5))
print(round(-1.5))

### round ndigits为负数

print(round(16323232, -1)) ## 只做舍入操作，在相应的十位、百位等上
print(round(23233232, -2))
```

## 精确浮点数计算

```
a = 4.2
b = 2.1
print(a + b)
print((a + b) == 6.3)

from decimal import Decimal, localcontext
a = Decimal('1.3')
b = Decimal('1.7')
print(a + b)
print(a / b)

with localcontext() as ctx:
    ctx.prec = 3
    print(a/b)

## 在正常的科学计算中，可以使用python的标准库进行计算，速度快，误差基本可以忽略，但是有的时候还是要注意误差，以及注意数学运算的使用， 比如：


num = [1.2e+18, 1, -1.2e+18]
print(sum(num))
import math
print(math.fsum(num))
```
## Number Format

```
a = 1234.56789
format(a, '.2f') ## 用round方式进行四舍五入
format(a, '<10.2f')
format(a, '>10.2f')
format(a, '^10.2f')
format(a, ',')
format(a, '0,.1f')

### exp format

format(a, 'e')
format(a, '0.2e')

### format digit in a string

'The value is {:10,.1f}'.format(a)

### In all, width + digits: `[<>^]?width[,]?(.digits)?[ef]?`
print(format(a, '.2'))
```

## 不同进制

```
a = 1234
bin(a)
format(a, 'b') ## Starts with '0b'

oct(a)
format(a, 'o') ## Starts with '0o'

hex(a)
format(a, 'x') ## Starts with '0h'

int('4d2', 16)
int('10101010101', 2)
```

## 字节字符串与大整数的打包与解包

```

data = b'\x00\x124V\x00x\x90\xab\x00\xcd\xef\x01\x00#\x004'
print(int.from_bytes(data, 'little'))
print(int.from_bytes(data, 'big'))

x = 94522842520747284487117727783387188
print(x.to_bytes(16, 'little'))
print(x.to_bytes(16, 'big'))
```

## 复数的数学运算

```
### 复数表示
import math
import cmath ## python 标准库不支持复数的运算
import numpy as np

a = 3 + 5j
b = complex(3, 5)
print(a, b)
print(a.real, a.imag, a.conjugate)
print(b.real, b.imag, b.conjugate)
math.sin(3)
print(cmath.sin(a))

# print(math.sqrt(-1))
print(cmath.sqrt(-1))

array = np.array([1 + 2j, 2 + 3j])
print(np.sum(array + 2))
```

## Nan, inf

```
a = float('nan')
d = float('nan')

print(a == d)
b = float('inf')
c = float('-inf')

import math
print(math.isinf(b))
print(math.isinf(c))
print(math.isnan(a))
```
## 分数运算

```
from fractions import  Fraction
a = Fraction(3, 4)
b = Fraction(4, 5)
c = a + b
print(c)
print(c.numerator, c.denominator)

print(float(c))
d = 3.75
print(d.as_integer_ratio())
num, den = d.as_integer_ratio()
print(Fraction(*d.as_integer_ratio()))
print(Fraction(num, den))
```

## Reference

- [Python Cookbook](http://python3-cookbook.readthedocs.io/zh_CN/latest/c01/p11_naming_slice.html)
