title: 在Markdown中插入数学公式 
date: 2015-09-25 00:17:43
tags: Mathjar
categories: Mathjar
description: 在Markdown中插入数学公式
mathjax: true
---

关于如何在hexo中插入数学公式，在网上找了一些方法，这里列出两个我觉得比较好的方法。

# 使用mathjax引擎

这是一种比较简单的方法，直接在markdown中插入


```
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script> 
```


即可，不需要额外的配置，方便快捷。还有一个很大的好处就是在macdown中打入公式后，在macdown的右侧会渲染出相应的公式，便于查看。但是在hexo d到github后，这种方法响应的时间比较长，所以最好的办法是，在macdown中编辑的时候用这种方法，用hexo d上传的时候用下面的方法。

以下是几个相应的例子和代码：

**行间公式**：

$$A_1 = 5 $$

$$\frac{a}{b}$$

$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$

**行内公式**：


\\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)

**行间公式代码**：

```latex
$$A_1 = 5 $$

$$\frac{a}{b}$$

$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$

```
**行内公式代码**：

```
\\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)
```

# 对主题的after_footer.ejs作修改

具体流程请参见[在 Hexo 中完美使用 Mathjax 输出数学公式][url]

值得注意的是，使用这种方法不能在编辑macdown公式的时候实时进行查看，需要上传之后才可以看到相应的效果。还有这篇blog中提到的安装hexo math插件的方法，在mac上我没有成功设置成功，找不到相应的hexo math install指令。


薛定谔公式：

$$ i\hbar\frac{\partial \psi}{\partial t}
= \frac{-\hbar^2}{2m} \left(
\frac{\partial^2}{\partial x^2}
\+ \frac{\partial^2}{\partial y^2}
\+ \frac{\partial^2}{\partial z^2}
\right) \psi + V \psi.$$


# Reference

[在 Hexo 中完美使用 Mathjax 输出数学公式](http://lukang.me/2014/mathjax-for-hexo.html)

[Hexo上使用MathJax来实现数学公式的表达](http://hijiangtao.github.io/2014/09/08/MathJaxinHexo/)


# PS
Hexo中代码显示的效果还是不错的，以python快排为例：


```python
class Solution(object):
    """docstring for Solution"""
    def quick_sort(self,num):
        if len(num) <= 1:
            return num
        pivot = num[len(num)/2]
        left = [i for i in num if i < pivot]
        middle = [i for i in num if i ==pivot]
        right = [j for j in num if j > pivot]
        return self.quick_sort(left) + middle + self.quick_sort(right)
        
```
用macdown书写markdown，体验很不错。

![macdown](/img/macdown.png)

[url]: http://lukang.me/2014/mathjax-for-hexo.html


<!--<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script> -->

<!--我是注释－－－-->

<!--mathjax: true-->
<!--段落代码：每行文字前加4个空格或者1个Tab-->