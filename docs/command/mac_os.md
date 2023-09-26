---
layout: default
title: mac os
parent: Command
nav_order: 10
---

# 1. Homebrew

## 1.1. Command
```shell
brew -v 
brew search jdk
brew install
brew uninstall
brew list

brew doctor

```

## 1.2. 镜像加速

- https://zhuanlan.zhihu.com/p/137464385

## 1.3. Jdk

- 查看本地安装的java版本  /usr/libexec/java_home -V

- mac上的默认目录 /Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home

```shell

brew install homebrew/cask-versions/openjdk@17

# 跳转到根目录显示查看所有.a配置文件
cd
ls -a

# 添加java_home到.bash_profile文件中
touch .bash_profile
# 使用vim编辑器编辑 .bash_profile文件
vi .bash_profile

# 添加下面代码
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$JAVA_HOME/bin:$PATH
export CLASS_PATH=$JAVA_HOME/lib

# 添加完后点按esc(确认输入无效)后输入“:wq”(没引号)
是配置生效
$source .bash_profile

echo "source ~/.bash_profile" >>~/.zshrc

#------------------------
# JDK 13
JAVA_13_HOME=/Library/Java/JavaVirtualMachines/jdk-13.0.1.jdk/Contents/Home/
# JDK 8
JAVA_8_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/# 默认JDK8
export JAVA_HOME=$JAVA_8_HOME# alias命令动态切换JDK版本
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
alias jdk13="export JAVA_HOME=$JAVA_13_HOME"export PATH=$JAVA_HOME/bin:$PATH:.
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
$ source ~/.bash_profile #Zsh应改为 source ~/.zshrc

```

# 2. Tabby 远程登陆

# 3. SCROLL REVERSER

用于鼠标与触摸板的转换

# 4. Logi Optional 罗技鼠键