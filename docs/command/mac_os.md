---
layout: default
title: mac os
parent: Command
nav_order: 10
---

# Homebrew

## 镜像加速
- https://zhuanlan.zhihu.com/p/137464385

## jdk 
安装的目录
- mac上的默认目录 /Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home
```shell
//跳转到根目录显示查看所有.a配置文件
//跳转到根目录显示查看所有.a配置文件
 $cd
 $ls -a
 
 添加java_home到.bash_profile文件中
 $ touch .bash_profile  创建一个配置文件(如果存在就打开)
   //使用vim编辑器编辑 .bash_profile文件
 $ vi .bash_profile  
 
 //添加下面代码
 export JAVA_HOME=$(/usr/libexec/java_home)
 export PATH=$JAVA_HOME/bin:$PATH
 export CLASS_PATH=$JAVA_HOME/lib
 
 添加完后点按esc(确认输入无效)后输入“:wq”(没引号)
 //是配置生效
 $source .bash_profile
 
 echo "source ~/.bash_profile" >> ~/.zshrc
```