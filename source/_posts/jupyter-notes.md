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


## Reference 

- [reference link](https://www.jianshu.com/p/afea092dda1d)
