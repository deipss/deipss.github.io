---
layout: default
title: conda
parent: Command
nav_order: 1
---

# 1. ~~安装显卡驱动~~

## 1.1. 预备依赖软件

```shell
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install build-essential libc6:i386
sudo apt-get install dkms build-essential linux-headers-generic

```

## 1.2. 先卸载原有N卡驱动

```shell

#for case1: original driver installed by apt-get:
sudo apt-get remove --purge nvidia*
#for case2: original driver installed by runfile:
sudo chmod +x *.run
sudo ./NVIDIA-Linux-x86_64-384.59.run --uninstall

```

## 1.3. 禁用自带的 nouveau nvidia驱动

```shell

sudo gedit /etc/modprobe.d/blacklist.conf
#并添加如下内容:
blacklist nouveau
options nouveau modeset=0
#再更新一下
sudo update-initramfs -u
#修改后需要重启系统。确认下Nouveau是已经被你干掉，使用命令：
lsmod | grep nouveau
#如果没有屏幕输出，说明禁用nouveau成功。

```

## 1.4. 禁用X-Window服务

``` shell

#这会关闭图形界面，但不用紧张
sudo service lightdm stop 
Ctrl-Alt+F1进入命令行界面，输入用户名和密码登录即可。
在命令行输入：sudo service lightdm start ，然后按Ctrl-Alt+F7即可恢复到图形界面

```

## 1.5. 命令行安装驱动

```shell

sudo chmod +x NVIDIA-Linux-x86_64-384.59.run
sudo ./NVIDIA-Linux-x86_64-384.59.run –no-x-check -no-nouveau-check -no-opengl-files

```

- no-opengl-files：表示只安装驱动文件，不安装OpenGL文件。这个参数不可省略，否则会导致登陆界面死循环，英语一般称为”login
  loop”或者”stuck in login”。
- no-x-check：表示安装驱动时不检查X服务，非必需。
- no-nouveau-check：表示安装驱动时不检查nouveau，非必需。
- Z, --disable-nouveau：禁用nouveau。此参数非必需，因为之前已经手动禁用了nouveau。
- A：查看更多高级选项。

# 2. ~~安装显卡-另一个版本~~

## 2.1. 安装显卡驱动

```shell


预备依赖软件

sudo dpkg --add-architecture i386
sudo apt update
sudo apt install build-essential libc6:i386
sudo apt-get install dkms build-essential linux-headers-generic

先卸载原有N卡驱动

#for case1: original driver installed by apt-get:
sudo apt-get remove --purge nvidia*

#for case2: original driver installed by runfile:
sudo chmod +x *.run
sudo ./NVIDIA-Linux-x86_64-384.59.run --uninstall

禁用自带的 nouveau nvidia驱动

sudo gedit /etc/modprobe.d/blacklist.conf

并添加如下内容:
blacklist nouveau
options nouveau modeset=0

再更新一下
sudo update-initramfs -u


修改后需要重启系统。确认下Nouveau是已经被你干掉，使用命令： 
lsmod | grep nouveau
如果没有屏幕输出，说明禁用nouveau成功。
禁用X-Window服务

#这会关闭图形界面，但不用紧张
sudo service lightdm stop 

Ctrl-Alt+F1进入命令行界面，输入用户名和密码登录即可。

在命令行输入：sudo service lightdm start ，然后按Ctrl-Alt+F7即可恢复到图形界面
命令行安装驱动


sudo chmod +x NVIDIA-Linux-x86_64-384.59.run
sudo ./NVIDIA-Linux-x86_64-384.59.run –no-x-check -no-nouveau-check -no-opengl-files
no-opengl-files：表示只安装驱动文件，不安装OpenGL文件。这个参数不可省略，否则会导致登陆界面死循环，英语一般称为”login loop”或者”stuck in login”。

no-x-check：表示安装驱动时不检查X服务，非必需。

no-nouveau-check：表示安装驱动时不检查nouveau，非必需。

Z, --disable-nouveau：禁用nouveau。此参数非必需，因为之前已经手动禁用了nouveau。

A：查看更多高级选项。


```

## 2.2. 参考资料

https://blog.csdn.net/CosmosHua/article/details/76644029
https://blog.csdn.net/u014682691/article/details/80605201

## 2.3. 北师大镜像

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