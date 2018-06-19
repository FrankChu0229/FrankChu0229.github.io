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

## String 查找和替换

str中的replace只能进行字面的查找替换，更复杂的可以使用re.sub(), re.sub()支持正则
```
import re
text = 'yeah, but no, but yeah, but no, but yeah'
print(text.replace('yeah', 'yep'))

print(re.sub(r'y.*h', 'hello', text))

text = 'Today is 11/27/2012. PyCon starts 3/13/2013.'
pattern = re.compile(r'(\d+)/(\d+)/(\d+)')
sub_text = pattern.sub(r'\3-\1-\2', text) ## subn 执行sub，同时返回替换次数n
print(sub_text)

## sub(pattern, repl, string), repl 可以是string和callable， 当是callable的时候，传入参数为match object，例如：
from calendar import month_abbr
def change_date(m):
    mon_name = month_abbr[int(m.group(1))]
    return '{} {} {}'.format(m.group(2), mon_name, m.group(3))

pattern.sub(change_date, text)

```

## Re 匹配模式

### 匹配时忽略大小写
```
text = 'UPPER PYTHON, lower python, Mixed Python'
print(re.findall('python', text, flags=re.IGNORECASE))
print(re.subn('python', 'snake', text, flags=re.IGNORECASE))

def matchcase(word):
    def replace(m):
        text = m.group()
        if text.isupper():
            return word.upper()
        elif text.islower():
            return word.lower()
        elif text[0].isupper():
            return word.capitalize()
        else:
            return word
    return replace

print(re.subn('python', matchcase('snake'), text, flags=re.IGNORECASE))

```

### 最短匹配模式

默认情况下，正则匹配都是最长匹配，通过修改* 为*?, 可以将贪婪模式改编成非贪婪模式，即最短匹配。
```

text = 'Computer says "no." Phone says "yes."'
print(re.findall(r'".*"', text))
print(re.findall(r'"(.*)"', text)) ## 捕获分组
print(re.findall(r'"(?:.*)"', text)) ## 非捕获分组
print(re.findall(r'"(.*?)"', text)) ## 最短匹配


```

### 多行匹配

在正则表达式中， `.` 不能匹配包含"\n"的换行符，所以要想进行多行匹配，又以下两种方式：
```
text = 'hello\nhshsh'
print(re.match(r'.*', text))
print(re.match(r'(?:.|\n)*', text))

pattern = re.compile('.*', re.DOTALL)
print(pattern.match(text))

```
