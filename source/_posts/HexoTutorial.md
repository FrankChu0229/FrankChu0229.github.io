title: Hexo Tutorial
date: 2015-09-24 11:24:01
tags: Hexo 
categories: Tutorial
description: Hexo setup tutorial
---

# Hexo——Mac安装教程

在mac上搭建我的hexo博客还是折腾了一小会的，在这里写下这篇教程好让后人乘凉。

## 环境准备

安装hexo需要先安装git和node.js，按照网上找的教程，在命令行里面进行安装，这样子安装会遇到很多error，需要改一些路径，很麻烦，我就是在这个地方消耗了很多时间。

推荐的安装方法就是去node.js和git的官网，下载mac对应的安装包，安装完成即可，无需做改动，方便迅速。

**传送门**：

[node.js官网](https://nodejs.org/en/) 

[git官网](http://git-scm.com/download/mac)

这里需要提到的一点是，对下载好的node.js安装包进行安装的时候，同时会安装好npm。如果是在命令行模式下安装npm的话，会遇到很多问题，比较耗时，也比较让人烦。

## 安装Hexo

这里可以用node -v 和 git --version 来查看node.js和git是否已经安装好。

如果上面的应用程序都安装好了的话，就可以准备安装Hexo了。

`$ npm install -g hexo-cli`

安装Hexo完成后，执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

`$ hexo init <folder>` // `hexo init ~/Hexo`

`$ cd <folder>`     //`cd Hexo/`

`$ npm install`    

*这里需要注意的是不要忘记cd到你指定的文件夹下，不要忘记 npm install.*


## 小试牛刀

以上都安装好后，cd到安装Hexo的文件夹，输入

`$ hexo g `

`$ hexo s`

然后在浏览器中打开[http://0.0.0.0:4000/](http://0.0.0.0:4000/)这个链接，就能看到你的博客了。

## 遇到的问题
在调用hexo d上传的时候，会遇到**ERROR Deployer not found: git**的问题， 解决方法：**npm install hexo-deployer-git --save**即可。



## 友情链接
[Hexo官方文档](https://hexo.io/zh-cn/docs/setup.html)







--- 