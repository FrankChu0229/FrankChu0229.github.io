title: Numpy Summary
date: 2018-06-06 17:42:23
tags: [python, numpy, dl]
categories: Machine Learning
description: Numpy Summary.
---

Numpy 是一个高性能的多维数组计算库，常被用在科学计算中。

## Numpy Fields and Basic Operations

Numpy basic fields: `ndim`, `size` and `shape`.

```
## Numpy basics: fields and matrix operation.

import numpy as np

array = np.array([[1,2,3], [4,5,6], [7,8,9]])
a = np.array([1,2,3])
print(a.ndim)
print(array)
print(array[0][2])
print('dim is', array.ndim)
print('shape is', array.shape)
print('size is', array.size)

## dtype 

dtype_array = np.array([1,2,3])
print(dtype_array.dtype)
dtype_array1 = np.array([1,2,3,4,5], dtype=np.float)
print(dtype_array1.dtype)

## matrix operation

matrix_a = np.array([[1,2,3], [4,5,6]])
print(matrix_a)
matrix_zeros = np.zeros((2,3))
print(matrix_zeros)
matrix_ones = np.ones((2,3), dtype=int)
print(matrix_ones)
matrix_empty = np.empty((2,3)) ## create values that are close to zero.
print(matrix_empty)
matrix_arange = np.arange(1,50,2)
print(matrix_arange)
matrix_reshape = matrix_arange.reshape((5,5))
print(matrix_reshape)
## linspace: to create 20 elememts between 1 and 10 
matrix_linspace = np.linspace(1,10,20)
print(matrix_linspace)

```

## Numpy Array Computation

```
import numpy as np

## numpy calculation:

matrix_a = np.array([[1,2,3], [4,5,6], [7,8,9]])
matrix_b = np.arange(10, 19).reshape((3,3))

print(matrix_a)
print(matrix_b)
print(matrix_a + matrix_b)
print(matrix_a - matrix_b)
print(matrix_a * matrix_b) ## point-wise multiply.
print("a*b for matrix multiply calculation is:", np.dot(matrix_a, matrix_b)) ## normal matrix multiply
print("a*b for matrix multiply calculation is:", matrix_a.dot(matrix_b)) ## normal matrix multiply
print(np.sin(matrix_a))
print(matrix_b < 15)
print(matrix_b**2)

matrix_random = np.random.random((3,4))
print(matrix_random)

## Operation over the whole matrix.
print(np.sum(matrix_random))
print(np.min(matrix_random))
print(np.max(matrix_random))

## Operation over the single row or column， 0 for column, 1 for row.
print('row operation results:', np.max(matrix_random, axis=0))
print('column operation results:', np.max(matrix_random, axis=1))

## numpy mateix operation 2

import numpy as np

matrix = np.arange(12, 0, -1).reshape((3,4))
print(matrix)

print(matrix.argmax())
print(matrix.argmin())
print(np.argmax(matrix))

print(matrix.mean())
print(matrix.cumsum()) ## cumulutive sum
print(np.diff(matrix)) ## 后一项减去前一项

print(np.nonzero(matrix))
print(matrix)
print(np.sort(matrix, axis=0))
print(np.sort(matrix, axis=1))

print(matrix.T)
print(np.transpose(matrix))

print(np.clip(matrix, 5,9))

```

## Numpy 索引

```
## numpy 索引

import numpy as np

matrix = np.arange(12).reshape((3,4))
print(matrix)

print(matrix[0][1])
print(matrix[0,1])

print(matrix[0][1:3])
print(matrix[0, 1:3])

for row in matrix:
    print(row)
    
for column in matrix.T:
    print(column)

print(matrix.flatten()) ## list

print(list(matrix.flat)) ## 迭代器

```

## Numpy Array 合并 && 分割

```
## Numpy array 合并

a = np.array([[1,2,3],[4,5,6]])
b = np.array([[4,5,6],[1,2,3]])

c = np.vstack((a,b))
print(c)

d = np.hstack((a, b))
print(d)

tmp = np.array([1,2,3])
print(tmp.shape)
print(tmp[:, np.newaxis].shape)
print(tmp[np.newaxis, :].shape)


print(np.hstack((a,b)))
print(np.concatenate((a, b), axis=1))
print(np.vstack((a,b)))
print(np.concatenate((a,b), axis=0))

## numpy array 分割

## 等分分割
print(c)
c_split = np.split(c, 2, axis=0)
print(np.vsplit(c, 2))
print(c_split)

print(d)
d_split = np.split(d, 2, axis=1)
print(np.hsplit(d, 2))
print(d_split)



## 不等分分割

not_split = np.array_split(c, 3, axis=0)
print(not_split)

```

## Numpy Copy and Deep Copy

```
## numpy copy and deep copy
a = np.array([[1,2,3]])
print(a)
b = a
print(b is a)
a[0][1] = 5
print(a)
print(b)

## deep copy
c = a.copy()
print(a)
print(c)

a[0][1] = 8
print(a)
print(c)
```

---

## Reference

- [reference-mofan-numpy-tutorial](https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/)
