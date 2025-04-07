---
layout: default
title: conda
parent: Command
nav_order: 1
---


## 1. 参考资料

https://blog.csdn.net/CosmosHua/article/details/76644029
https://blog.csdn.net/u014682691/article/details/80605201

## 2. 北师大镜像

```bash
  conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/main
  conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/r
  conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/msys2
  
  
  conda config --remove channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/main
  conda config --remove channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/r
  conda config --remove channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/msys2
  
  conda config --set proxy_servers.https socks5://127.0.0.1:10808

```

# 3. 安装环境

- 官方路径 ：https://docs.anaconda.com/free/anaconda/install/linux/
- anaconda 的版本信息：https://repo.anaconda.com/archive/

```shell
conda create -n py36 python=3.6
conda create -n py310 python=3.10.14
# 显示当前的环境
conda info -e 
conda env list

activate [env_name]
deactivate

conda create [env_name]
source activate py36 
source activate py310 
source deactivate


```

## jupyter安装与远程登陆

- 远程登陆参考文档 https://www.jianshu.com/p/8fc3cd032d3c

```shell

conda install jupyter notebook

vim /home/deipss/.jupyter/jupyter_notebook_config.py
c.NotebookApp.ip='*'
c.NotebookApp.password = u'argon2:$argon2id$v=19$m=10240,t=10,p=8$VH3vhkEL5tQMKg6FWYWTeQ$9U2v6D8llgrrEIeiwAqiew'
c.NotebookApp.open_browser = False
c.NotebookApp.port =7888 #可自行指定一个端口, 访问时使用该端口

jupyter notebook

```

## comfyUI

### 后台安装一些pip的包

- nohup sh -c 'pip install -r /home/deipss/jupyter_files/ComfyUI/requirements.txt' > runoob.log 2>&1 &

### 3.1. 参考资料

- [https://blog.csdn.net/CosmosHua/article/details/76644029](https://blog.csdn.net/CosmosHua/article/details/76644029)
- [https://blog.csdn.net/u014682691/article/details/80605201](https://blog.csdn.net/u014682691/article/details/80605201)