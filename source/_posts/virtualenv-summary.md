title: Virtualenv Summary
date: 2018-09-27 14:59:03
tags: [python, virtualenv, tools]
categories: Python Env Management.
description: Virtualenv Summary.
---

## Introduction
Conda 在使用时还是稍微有些重，virtualenv可以对每一个项目创建一个虚拟环境，然后对每一个项目的python环境进行隔离。

## Usage

- `pip install virtualenv` to install
- `virtualenv ENV` to create virtual env in the directory `ENV`, which will create `bin`, `lib`, `include`.
- `source bin/activate` to activate the current env.
- `deactivate` to deactivate the current env.
- `rm -rf ENV` to rm the current virtual env.

## Reference

- [User Guide](https://virtualenv.pypa.io/en/stable/)
