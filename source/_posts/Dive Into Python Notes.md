title: Dive Into Python3 Notes Your First Program
date: 2017-10-15 16:15:23
tags: [Python, notes, summary]
categories: python
description: Dive Into Python3 Notes - Your First Program
---

# Dive Into Python3 Notes - Your First Program

## Define Functions

- python function中没有定义返回类型。 每一个python functiuon都会有一个返回值，如果function中含有return value， 则返回相应的值；否则返回`None`(Python中的null)
- 在python中， 变量(anything)从不显示声明类型，python 内部会自动track相应的类型，内部根据该变量第一次被赋予的类型来决定该变量的类型。
- python function允许参数有默认值，如果该参数没有被输入，则使用该参数的默认值；另外，function参数的指定，可以不按照在function中定义的顺序，只要提供该function中的变量名字即可。

## Writing Readable Code

### Documentation Strings (doc-string for short)

- """ """ triple quotes signify a multi-line doc-string. 
- Doc-string 必须在相应函数(类)中最前的地方被定义，在Python中， everything is object， doc-string 可以通过该object的.__doc_string得到。

## Python Import Path

-  在Python中，当你想import其他python module的时候，python会首先去几个地方进行查找。 Specifically, python会在`sys.path`中的所有目录下进行查找。
-  `sys.path` 是一个list， 你可以像操作正常的list一样将你的python module加入到`sys.path`中，例如：

```
import sys
sys.path.insert(0, "./tmp/hello_world.py")

```

## Everything Is Object In Python

- Python module is a object, so when you want to use the public functions, classes or attributes in the module, you need to add the module name before that, e.g., `FirstProgram.HelloWorld()`
- Python function is also a object, so you can use `FunctionName.__doc__` to get its doc-string.
- In python, `classes`, `functions`, `modules` are first-class objects, which means you can pass them as a parameter in a function.

## Python Indenting Code

- 在python中，函数没有显式的begin、end标志，也没有像java等语言用{}来标记函数的start和end。Python中用`冒号:`和`缩进`。
- 可以这样简单的理解，像在java中，涉及到code block的地方，需要有{}来标记，例如if， for， while， function等；在python中对于这些部分需要有`:`和缩进

## Exceptions In Python

- If you’re opening a file, it might not exist. If you’re importing a module, it might not be installed. If you’re connecting to a database, it might be unavailable, or you might not have the correct security credentials to access it. If you know a line of code may raise an exception, you should handle the exception using a `try...except` block, and `raise` to generate exceptions.
- 例如：

```
try: 
	import chardet
except ImportError:
	chardet = None

```

## In Python, everything is case-sensitive

## Running Scripts

Python modules are objects and you can use some attributes of them:

- all modules have an attribute: `__name__`. 如果你是import a module， `__name__`即为该module的filename (不带路径名)；但如果你将该module单独的去run，此时`__name__`的值为它的default值：`__main__`


--- 