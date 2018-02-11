title: Cuda Notes
date: 2018-02-08 15:32:14
tags: [ubuntu, notes, cuda, DL]
categories: notes
description: CUDA Notes
---

## Nvidia CUDA 9.1 Install
- download **runfile** from [here](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal)
- install according to the [instructions](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal)
- add the path, i.e. PATH and LD_LIBRARY_PATH, to your `~/.bashrc` like this:

```
export PATH=$PATH:/usr/local/cuda-9.1/bin/
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.1/lib64

```

## NVCC Usage
- show version: `nvcc --version`
- show help: `nvcc --help`


## 查看nvidia gpu 使用情况
- `nvidia-smi`








---
