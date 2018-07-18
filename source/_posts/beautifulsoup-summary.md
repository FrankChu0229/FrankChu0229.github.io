title: Beautiful Soup Summary
date: 2018-06-08 14:52:12
tags: [python, beautiful-soup, spider]
categories: spider
description: Beautiful Soup Summary.
---

## Four Python Object Type:

```
from bs4 import BeautifulSoup
import lxml

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html, 'lxml') ## will parse the html into a tree structure, and each node is a python object, and the python object can be categorized into the following four type:
# print(soup.prettify())

## Tags: name and attrs
print('html is:', soup.html)
print('head is: ', soup.head)
print('title is: ', soup.title)
print('p is:', soup.p) ## the first tag
print('a is:', soup.a) ## the first tag
print('Type is:', type(soup.a))

### Two fields in Tag: name and attribute.
print('soup name', soup.name)
print('p tag name', soup.p.name) ## name p
print('p tag attributes', soup.p.attrs) ## attrs {'class': ['title'], 'name': 'dromouse'}

print('html node name', soup.html.name)
print('html node attrs', soup.html.attrs)

print("Get attributes of p", soup.p['class'])
print("Get attributes of p", soup.p.get('class'))
soup.p['class'] = ['hihi']
print("Get attributes of p after changing attrs", soup.p['class'])

## NavigableString: get the values in the tag
print("Get the value in the tag", soup.p.string)
print('Type of the value in the tag', type(soup.p.string))

## BeautifulSoup Object: 表示一个文档的全部内容，可以当作一个Tag对象, root node?
print(type(soup))
print(soup.name)
print(soup.attrs)

## Comment: 本质还是一个NavigableString, 可以使用.string 获得注释内容，但不会由注释符号

print(soup.a)
print(type(soup.a))
print(soup.a.name)
print(soup.a.string)

```

## Reference
- [reference](https://cuiqingcai.com/1319.html)
