---
title: conda的简单使用
date: 2020-10-24 15:09:59
---
### Linux设置默认加载环境

`conda config --set auto_activate_base fals`

### 修改python3为系统默认的Python
vim .bashrc，添加`alias python3="/usr/bin/python3.8"`
### 激活虚拟环境base
`conda activate base`
### 退出虚拟环境
`conda deactivate`
### 创建虚拟环境
`conda create -n pytorch`
这样就创建了一个名为pytorch的虚拟环境，-n表示name
<!--more-->
### 删除虚拟环境

`conda remove -n pytorch --all`
### 查看conda所有的虚拟环境
`conda env list`

### 查看安装的包
激活环境之后查看的是某个环境下安装的所有的包，默认是查看base环境的包
`conda list`
指定某个环境下的所有包,查看pytorch这个环境下所有的包
`conda list --n pytorch`

### 在虚拟环境安装包

没有激活虚拟环境，需要指定-n来说明安装的虚拟环境，这里在pytorch环境中安装torchvision包

`conda install -n pytorch torchvison`

当激活了虚拟环境之后，只用直接install即可

`conda install torchvision`

### conda设置代理
编辑`~/.condarc`文件, 设置conda代理。清华的源不太行，总是下载失败。

```
proxy_servers:
   http: http://127.0.0.1:8889
   https: https://127.0.0.1:8889
```

### 在anaconda中使用Jupyter notebook
> 安装jupyter
> 激活环境(pytorch), pip install jupyter
> 安装成功，启动就好，jupyter notebook
1. 在base环境中安装ipykernel

   `conda install ipykernel`

2. 激活虚拟环境pytorch

   `conda activate pytorch`

3. 为虚拟环境下创建ipykernel文件

   `conda install ipykernel`
4. 将虚拟环境中的解释器写入jupyter notebook的ipykernel
   `python -m ipykernel install --user --name  pytorch_虚拟环境的名字`
5. 激活虚拟环境或者不激活启动jupyter notebook都可以使用相关服务

### 搜索包

`conda search -t torchtext`

搜索torchtext包，例如搜索结果为

![conda_search_result](https://github.com/georgezzzh/resource/raw/master/conda/conda_search.png)

### 安装搜索的包

把conda.anaconda.org/后面的内容换为图中的第一列
* 安装torchtext
` conda install -c https://conda.anaconda.org/pytorch torchtext`
