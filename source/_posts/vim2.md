title: Vim Notes
date: 2016-11-21 17:00:02
tags: [vim, notes]
categories: vim notes
description: vim 进阶
---

# 多行编辑

用`vim`进行多行编辑是十分方便的，只需要以下步骤：

 1. `Ctrl-v` 进入纵向编辑模式 
 2. 通过`Shift-g`, `gg`, `j`,`k`等选择要编辑的行
 3. 若要删除多行，则`d`即可; 若要编辑多行的内容，`Shift-i`进入INSERT模式
 4. 进行编辑，然后按两次`ESC`就会发现多行编辑完成。


# 分屏

在使用`vim`编辑器的时候，结合`vim`的分屏功能，可以使得编辑更加高效：

## 进入分屏的几种方式：

 1. `vim -On file1 file2` 将`file1`和`file2`进行垂直分屏
 2. `vim -on file1 file2` 将`file1` 和`file2`进行水平分屏
 3. `vim file` 然后通过：
	- `Ctrl-w s`对当前文件进行水平(上下)分屏
	- `Ctrl-w v`对当前文件进行垂直(左右)分屏
	- `:sp newFile` 打开新的文件，上下分割 
	- `:vsp newFile` 打开新的文件，左右分割

## 分屏的切换：

 1. `Ctrl-w j` 切换到下边的分屏上
 2. `Ctrl-w k` 切换到上边的分屏上
 3. `Ctrl-w h` 切换到左边的分屏上
 4. `Ctrl-w l` 切换到右边的分屏上

## 分屏的关闭：

 1. `Ctrl-w c` 关闭当前分屏
 2. `Ctrl-w q` 关闭当前分屏，若为最后一个分屏，则退出`vim`.

用分屏是很方便的，当用`:sp newFile`打开一个新的文件时，可通过`y`, `p`的方式进行复制、粘贴，大大提高效率。

## vim delete part of lines
`:%s/": {//`

## delete the whole line satisfied with the pattern:
`:g/^9 /d`

# 展开和折叠

- `zc` 对当前折叠
- `zC` 对当前所在区域折叠
- `zo` 对当前展开折叠
- `zO` 对当前所在区域展开折叠

--- 
