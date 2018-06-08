title: Pandas Summary
date: 2018-06-06 17:42:45
tags: [python, pandas, dl]
categories: Machine Learning
description: Pandas Summary.
---

## Pandas Demo

Pandas 是基于numpy构建的，如果用python中的list和dict来做比较，numpy可以看作是list， pandas为dict。

## Pandas Basic

### Series and DataFrame

```
import pandas as pd
import numpy as np

series = pd.Series([0,1,2,3,4,np.nan], index=['a', 'b', 'c', 'd', 'e', 'f'])
print(s)
print(s[0])

dates = pd.date_range('20180101', periods=6)
print(dates)
data_frame = pd.DataFrame(np.random.randn(6,4), index=dates, columns=['a', 'b', 'c', 'd'])
print(data_frame)
print(data_frame['b'])

df = pd.DataFrame(np.random.random((6,4)))
print(df)
print(df.dtypes)


df1 = pd.DataFrame(
    {
        'a': 1.,
        'b': pd.Categorical(['a', 'b', 'c', 'd']),
        'c': pd.Series(1, index=list(range(4))),
        'd': pd.Timestamp('20180101'),
        'e': np.array([4]*4)

    }
)
print(df1)
print(df1.index)
print(df1.columns)
print(df1.values)
print(df1.transpose)

print(df1.sort_index(axis=1, ascending=False))
print(df1.sort_values(by='b'))
```


## pandas choose data:
```
import pandas as pd
import numpy as np

dates = pd.date_range('20180101', periods=6)
df = pd.DataFrame(np.random.random((6,4)), index=dates, columns=['a', 'b', 'c', 'd'])
print(df)

## 多行多列
print(df['a'])
print(df.a)

print(df[0:2])
print(df['2018-01-02': '2018-01-03'])

## 使用loc iloc 和ix
## loc use index name
print('df loc values', df.loc['2018-01-02'])
print('df loc values', df.loc['2018-01-02', 'a'])
print('df loc values', df.loc[:,['a','b']])

## iloc use index:0,1,2,3
print('df iloc values', df.iloc[0,1])
print('df iloc values', df.iloc[0])
print('df iloc values', df.iloc[0, 1:3])
print('df iloc values', df.iloc[0:2, 1])
print('df iloc values', df.iloc[0:2, 1:3])
print('df iloc values', df.iloc[[1,2,3], 1:3])

print('df ix values:', df.ix[0:1, ['a', 'd']])

print('df comparision values are:', df[df.a > df.b])
```

## pandas set value

```

import pandas as pd
import numpy as np

dates = pd.date_range('20180101', periods=6)
df = pd.DataFrame(np.random.random((6,4)), index=dates, columns=['a', 'b', 'c', 'd'])

## 行列 loc iloc and
print(df)
df['a'] = 0
print(df)

df.b = 0
print(df)

## 有: 时 才是对行操作
df['2018-01-01': '2018-01-02'] = np.nan
print(df)

df.iloc[0,1] = np.nan
print(df)

df['e'] = np.nan
print(df)

df.b[df.b > -0.2] = 88
print(df)

## pandas nan process
print(df.dropna(axis=0, how='any')) ## axis=0 row, axis = 1 column
print(df.fillna(value=0))
print(df.isnull())
```

## pandas read and write file:
pandas can read and write csv, json, excel, html, pickle etc.
```
import pandas as pd

pd.read_csv('student.csv')
pd.read_excel('a.excel')

##
pd.to_pickle('student.pickle')

```

## Pandas Concat

```
import pandas as pd
import numpy as np

df1 = pd.DataFrame(np.ones((3,4))*0, columns=['a', 'b', 'c', 'd'])
df2 = pd.DataFrame(np.ones((3,4))*1, columns=['a', 'b', 'c', 'd'])
df3 = pd.DataFrame(np.ones((3,4))*2, columns=['a', 'b', 'c', 'd'])

print(pd.concat([df1, df2, df3], axis=0)) # 0 在row上concat
print(pd.concat([df1, df2, df3], axis=1)) # 1 在column上concat
print(pd.concat([df1, df2, df3], axis=0, ignore_index=True)) # ignore index: reindex

df4 = pd.DataFrame(np.ones((3,4))*2, columns=[ 'b', 'c', 'd', 'e'])

print(pd.concat([df1, df4], axis=0, join='outer'))
print(pd.concat([df1, df4], axis=0, join='inner'))

df5 = pd.DataFrame(np.ones((3,4))*2, columns=[ 'b', 'c', 'd', 'e'], index=[2,3,4])

print(pd.concat([df1, df5], axis=1))

print(pd.concat([df1, df5], axis=1, join_axes=[df1.index]))
print(pd.concat([df1, df5], axis=1, join_axes=[df5.index]))

## append 只能在行级别增加，即纵向增加

print(df1.append(df2))
print(df1.append([df2, df3]))
print(df1.append([df2, df3], ignore_index=True))

```
## pandas merge: 合并有相同column的pandas dataframe等
```
import pandas as pd
import numpy as np

a = pd.DataFrame({
    'a': ['a0', 'a1', 'a2'],
    'b': ['b0', 'b1', 'b2']
})


b = pd.DataFrame({
    'b' : ['b0', 'b1', 'b3']
})

print(pd.merge(a, b, on='b'))
print(pd.merge(a, b, on='b', how='outer'))

```

## Pandas Plot
```
%matplotlib inline

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


series = pd.Series(np.random.randn(1000))
print(series)
print(series.cumsum())
series.plot()
plt.show()

df = pd.DataFrame(np.random.randn(1000,4), columns=['a', 'b', 'c', 'd'])
df.plot()
plt.show()

```

## Reference

-[mofan-python-pandas-tutorial](https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/)
