title: Awk Summary
date: 2019-06-18 14:20:54
tags: [Linux. tool, awk] 
categories: Linux
description: Awk Summary.
---

# Awk Summary

`Awk` 命令可以帮助在linux命令行下更好的进行操作，提高效率.

## 基本用法

`awk 动作 文件名`

例如，`awk '{print $0 }' a.txt` 打印当前文件每一行，awk默认使用空格或tab进行分割，可以使用`$1`, `$2`等获取每一列的值。

```
$ awk '{print $0 }' a.txt
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62
```


如果想指定分割符，可以使用

```
awk -F ':' '{print $1}' a.txt
```

```
$  awk -F ' ' '{print $1}' a.txt
Marry
Jack
Tom
Mike
Bob
```

## Awk 中的变量

1. `$ + 数字` 获取相应列的值
2. `NF` 表示当前行有多少列，`$NF`, `$(NF-1)`获取倒数第二列
3. `NR` 表示当前处理的是第几行，相应的`FNR`表示的是在当前文件的行数
4. `FILENAME` 当前文件名
5. `RS` 行分割符，默认为换行符
6. `FS` 字段分割符， 默认为空格或者tab
7. `ORS` 输出行分割符， 默认为换行符
8. `OFS` 输出字段分割符，默认为空格

指定输出分割符例子：

```
awk 'NR!=1{print $1, $2, $3}' OFS="---" netstat.txt
```


```
$  awk 'NR!=1 {print $1, $2, $3}' OFS="---" a.txt
Jack---2321---66
Tom---2122---48
Mike---2537---87
Bob---2415---40
```

指定输入分割符例子: 

```
awk -F ':' '{print $1}' a.txt
```


## Awk中的函数

Awk 中可以使用如`toUpper()`等函数，例如

```
awk -F ':' '{print toupper($1)}' a.txt
```

```
$ awk -F ' ' '{print toupper($1)}' a.txt
MARRY
JACK
TOM
MIKE
BOB
```

其他函数：

- `tolower()`
- `length()`
等

## Awk中的条件

Awk 条件使用方式 `awk '条件 动作' 文件名`

例如：

`awk '/LISTEN/ {print $1}' a.txt`  当前行含有LISTEN字段, //中可以写正则

```
$ awk '/M.*y/ {print $0}' a.txt
Marry   2143 78 84 77
```

`awk '$6 ~ /LISTEN/ {print $1}' a.txt` 第六行是否含有LISTEN， ~表示模式开始，!~表示反模式 

```
$ awk '$1 ~ /M.*y/ {print $0}' a.txt
Marry   2143 78 84 77
```

`awk 'NR == 1 || NR > 3 {print $1}' a.txt` 

```
$ awk 'NR == 1 || NR > 3 {print $1}' a.txt
Marry
Mike
Bob
```

# Reference

- [https://coolshell.cn/articles/9070.html](https://coolshell.cn/articles/9070.html)
- [http://www.ruanyifeng.com/blog/2018/11/awk.html](http://www.ruanyifeng.com/blog/2018/11/awk.html)

