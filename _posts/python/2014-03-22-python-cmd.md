---
layout: post
title: python 常用命令
author: shefh
catalog:  true
header-img: img/home-mountain.jpg
tags:
    - python
---

## Conda install

```
brew install mimiconda
```

## Command

```
## Create a new virtual environment named dify
conda create --name dify python=3.10

## Activate the virtual environment
conda activate dify

## Deactivate current virtual environment
conda deactivate

## List all the virtual environment
conda env list	

## List all packages in the current environment
conda list	

## Search package
conda search --full-name <package_full_name>

## Remove virtual environment
conda remove -n dify --all	

## Export current environment
conda env export > environment.yaml

## Create an environment by a file
conda env create -f environment.yaml
```

###  Update conda source

```
## .condarc

channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

