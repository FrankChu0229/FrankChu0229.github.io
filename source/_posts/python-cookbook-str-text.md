title: Python Cookbook String and Text
date: 2018-06-04 16:12:53
tags: [python, coding, notes]
categories: python
description: Python Cookbook Notes String and Text.
---

## String Split

str split 不支持正则，比较复杂的情况考虑使用`re.split(regex, line)`

```

import re
line = 'asdf fjdk; afed, fjek,asdf, foo'
splits = re.split(r'[\s;,]\s*', line)
print(splits)
## ['asdf', 'fjdk', 'afed', 'fjek', 'asdf', 'foo']


## 需要注意的是 正则表达式的书写，根据场景决定是否使用包含括号的捕获分组

splits = re.split(r'(\s|;|,)\s*', line)
print(splits)
## ['asdf', ' ', 'fjdk', ';', 'afed', ',', 'fjek', ',', 'asdf', ',', 'foo']

## 如果不想将分隔符包括进去，但又想使用括号形式的正则，那么可以使用非捕获分组
splits = re.split(r'(?:;|\s|,)\s*', line)
print(splits)
## ['asdf', 'fjdk', 'afed', 'fjek', 'asdf', 'foo']

```

## String StartsWith && EndsWith

startswith 和 endswith 常常用来匹配字符串的首位模式，和生成式结合使用，常用来filter一个目录下制定格式的文件

```
import os
print(str.endswith('hello.txt', '.txt'))
print('hello.txt'.endswith('.txt'))

print(any([name.endswith(('.c', '.h')) for name in os.listdir('./')])) ## 只接受tuple形式的参数
print([name for name in os.listdir('./') if name.endswith(('.c', '.h'))])

```
## String Match and Search

str中的字符串查找只能是字面上的查找，如果想使用更复杂的功能，需要使用re中的方法

```
## str中常用的查找：find， startswith, endswith.
text = 'yeah, but no, but yeah, but no, but yeah'
print(text == 'yeah') ## exact match
print(text.startswith('yeah'))
print(text.endswith('yeah'))
print(text.find('no'))

## re 中常用到的字符串查找方法：match, search compile, findall, etc.
print(re.match(r'yeah.*no', text)) ## match 从字符串开始匹配, return match object, 提取第一个符合规律的对象
print(re.search(r'yeah.*no', text)) ## search 在整个字符串开始匹配, return match object, 提取第一个符合规律的对象
print(re.findall(r'yeah.*no', text)) ## return list

matcher = re.compile(r'yeah.*no') ## 如果pattern 多次用来匹配，则可以先compile出来
print(matcher.match(text)) 

datepat = re.compile(r'\d+/\d+/\d+')
text = 'Today is 11/27/2012. PyCon starts 3/13/2013.'
print(datepat.findall(text))

## 使用正则的捕获分组
datepat = re.compile(r'(\d+)/(\d+)/(\d+)')
print(datepat.findall(text)) ## finditer return 迭代器
m = datepat.search(text)
print(m.group(0))
print(m.group(1))
print(m.group(2))
print(m.group(3))
print(m.groups())

```
