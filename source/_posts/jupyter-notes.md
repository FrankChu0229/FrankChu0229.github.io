title: Jupyter Notebook Summary
date: 2018-05-21 20:47:37
tags: [python, conda, jupyter]
categories: [python, tools, conda, jupyter]
description: Jupyter notebook 使用总结.
---

## Jupyter Install 

- use `conda install jupyter`
- use `pip install jupyter`

## Use Jupyter On Gpu Server

- First, start jupyter-notebook on GPU
- Then, use `ssh myserver -L 8888:localhost:8888` to link local port `8888` to gpu server port `8888`


## Use Conda Virtual Environment On Jupyter

- install plugins on conda `conda install nb_conda`


## Jupyter Shortcut Keys:

### Command Mode

- press `esc` to enter command mode
- `x` cut cell
- `c` copy cell
- `v` copy cell to the downside
- `Shift + v` copy cell to the upside
- `a` create cell to the downside
- `b` create cell to the upside
- `dd` delete cell
- `ctrl + s` save jupyter file
- `shift + m` merge cell
- `m` change cell to markdown
- `y` change cell to code

### Edit Mode

- press `enter` to enter edit mode
- `tab` auto-completition
- `shift + tab` show instructions
- `ctrl + enter` run current cell
- `shift + enter` run current cell and go to next cell
- `alt + enter` run current cell and insert a new cell below


## Use Jupyter Extensions, e.g., vim

- First install `IPython-notebook-extensions` by 

```
$ pip install https://github.com/ipython-contrib/jupyter_contrib_nbextensions/tarball/master
$ jupyter contrib nbextension install --user

```

- Install Vim Extensions:

```
# 在 ~/.local/share/jupyter/ 建立nbextensions資料夾
$ mkdir -p $(jupyter --data-dir)/nbextensions
# Clone the repo
$ cd $(jupyter --data-dir)/nbextensions
$ git clone https://github.com/lambdalisue/jupyter-vim-binding

```

## Reference 

- [reference link](https://www.jianshu.com/p/afea092dda1d)

- [Vim Extensions](http://cyruschiu.github.io/2016/09/05/jupyter-notebook-vim-binding/)
