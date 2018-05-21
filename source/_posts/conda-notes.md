title: Conda Summary
date: 2018-05-21 20:59:20
tags: [python, conda]
description: Conda 使用总结.
---

## Conda Environment 

- list all conda envs: `conda env list`
- create conda env: `conda create --name test python=3.4`, if not specify python version, conda will ues the same python version as the python version in conda.
- conda activate env: `source activate env_name`
- conda deactivate env: `source deactivate env_name`
- create conda from file: `conda env create -f environment.yml`
- conda export environment config: `connda env export > environment.yml`
- conda remove an environment: `conda remove --name env_name --all`

## Conda Package

- conda search package: `conda search tensorflow`
- conda install package in a specific env: `conda install --name env_name tensorflow`
- conda install package in the current env: `conda install tensorflow=1.4.0`, if not given package version, the latest version will be uesd.
- conda list all packages in the current env: `conda list`
- conda update pacakge: `conda update tensorflow`
- conda remove package: `conda remove -n env_name tensorflow`
- if cannot find package by conda, you can use `pip install package_name`.

## Reference

- [Conda Documentation](https://conda.io/docs/user-guide/tasks/manage-environments.html#)
