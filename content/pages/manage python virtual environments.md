---
title: manage python environments
tags:
  - python
date created: Saturday, January 6th 2024, 21:36:53
date modified: Sunday, March 3rd 2024, 12:10:07
---

# Conda

The following section is to take notes of potential problems/solutions and/or corresponding commands when one tries to install python, conda and Python packages on osx-64/osx-arm64.

  

## Basic Knowledge about Conda

  

### Install Conda

  
- condaforge

- conda/miniconda

- mamba/micromamba

  

### Create a Conda Environment

  

The follow commands create an conda environment with name `<env-name>` and python of version `3.9`.

```shell
CONDA_SUBDIR=osx-64 conda create -n <env-name> python=3.9
conda activate myenv_x86
conda config --env --set subdir osx-64
```

### Install Packages via `conda-forge`

  

`c` stands for channel.

```shell
conda install <package> -c conda-forge
```

```shell
conda config --show [channels]
conda config --remove channels <channel>
conda config --add channels <channel>
conda config --set show_channel_urls yes
```

> [!tip] `veclib` speeds up `numpy` with ARM64 python
> result of Python 3.7.12(OSX64):
>
> ```python
> mean of 10 runs: 31.43530s
> ```
>
> result of Python 3.9.13(ARM64):
>
> ```python
> mean of 10 runs: 1.13074s
> ```

- python code

```python
import time
import numpy as np

np.random.seed(42)

a = np.random.uniform(size=(300, 300))
runtimes = 10
timecosts = []

for _ in range(runtimes):
  s_time = time.time()
  for i in range(100):
    a += 1
    np.linalg.svd(a)
  timecosts.append(time.time() - s_time)
print(f'mean of {runtimes} runs: {np.mean(timecosts):.5f}s')
```

## Using Conda via Mamba

  

# **Python Packaging and Dependency Management with Poetry**

  

> [!note] Reference
>  - https://github.com/python-poetry/poetry
>  - [https://blog.kyomind.tw/python-poetry/#從零開始使用-Poetry](https://blog.kyomind.tw/python-poetry/#%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E4%BD%BF%E7%94%A8-Poetry)
