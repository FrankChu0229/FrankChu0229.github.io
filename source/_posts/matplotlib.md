title: Matplotlib Summary
date: 2018-06-20 10:27:36
tags: [python, matplotlib, tools]
categories: Machine Learning
description: Matplotlib Summary.
---

## Matplotlib Basic Operations in Jupyter

```
import matplotlib.pyplot as plt
%matplotlib inline
import numpy as np

x = np.linspace(-1, 1, 50)
y = 2 * x + 1
plt.plot(x, y)
plt.show()

```

```
x = np.linspace(-1, 1, 50)
y1 = 2 * x + 1
y2 = 2 ** x + 1
plt.plot(x, y1)
plt.plot(x, y2, color='red', linestyle='--')
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.show()

```


## Reference

- [reference](https://blog.csdn.net/Notzuonotdied/article/details/77876080)
