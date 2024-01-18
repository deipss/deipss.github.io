---
layout: default
title: mac os
parent: Command
nav_order: 10
---

# 1. homebrew
- /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
## 1.1. command

```shell
brew -v 
brew search jdk
brew install
brew uninstall
brew list
# 诊断
brew doctor

# 清理不必要的安装包
brew cleanup

```

## 1.2. 镜像加速

- https://zhuanlan.zhihu.com/p/137464385

```shell

cd "$(brew --repo)"
git config -l 先查看下当前git的配置
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

env | grep homebrew
# 这次更新时间长，但更新完成后，会加速
brew update 

```

## 1.3. Jdk

- 查看本地安装的java版本 /usr/libexec/java_home -V

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

# JDK 13
JAVA_11_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.1.jdk/Contents/Home/
# JDK 8
JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home
export JAVA_HOME=$JAVA_8_HOME# alias命令动态切换JDK版本
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
alias jdk13="export JAVA_HOME=$JAVA_13_HOME"export PATH=$JAVA_HOME/bin:$PATH:.
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
$ source ~/.bash_profile 
#Zsh应改为 source ~/.zshrc

```

# 2. tabby 远程登陆

# 3. scroll reverser

用于鼠标与触摸板的转换

# 4. logi optional 罗技鼠键

# 5. cpu

用来操作计算单元的控制单元的指令集合就是CPU指令集，有X86、RAM、MIPS、IA64，可以分成复杂指令集和
精简指令集两类，CISC和RISC。
PC端主要是X86，移动端主是要RAM。

| 处理器体系结构的类型 | ARM          | x86/x64         |
|------------|--------------|-----------------|
| 架构开发公司	    | ARM	         | Intel /AMD      |
| 处理器制造公司	   | Intel,apple	 | Intel /AMD      |
| 指令集的架构	    | RISC	        | CISC            |
| 主要使用的用途	   | 智能手机，平板电脑	   | PC服务器           |
| 32位/64位	   | 两者都有	        | x86:32位/x64:64位 |
| 字节存储次序	    | bi-endian	   | Little endian   |

```shell
uname -pa
        
```

# 6. 目录

## 6.1. Mac 的文件目录结构

- /System 文件夹，系统文件夹。与Windows 之中的 C:\windows32 等文件夹类似。
  - Library 系统资料库，其中的 Caches 可以删除。
  - iOSSupport 提供了系统的 iOS 支持。
- /Applications GUI软件文件夹，共享的所有软件包都存放在此。
- /Library 应用资料库，包括了大部分非核心的系统组件。Caches 可删除。
- /Users 文件夹，与 Linux 之中的 /home 文件夹功能类似。而mac 之中的 /home 只是为了与 Linux 兼容，一般不放任何东西。
- /Network 和 /net 网络相关，空的。
- /Volumes 与 /mnt 类似，其中挂载了全部硬盘、网络硬盘等。
- /sbin，/bin，/usr /dev文件夹，与 Linux 基本一致。与 Linux 兼容。
- /etc, /var /tmp 文件夹，是位于 /private 之中对应文件夹的软连接。存放系统配置、数据库、缓存等。用于与 Linux 文件结构兼容。

## 6.2. Brew 软件位置

Mac 不自带包管理。但是可以很方便的获取 brew 包管理工具。brew 自身位于 /usr/local/Homebrew 目录。
其软件安装包没有安装在系统目录之下，而是位于 /usr/local/Cellar 里面。
举个例子，brew 安装的 Python3 位于 /usr/local/Cellar/python/3.7.1/ 之中，而 go 则可能位于 /usr/local/Cellar/go/1.12.1 之中。

但是这些软件位置互相独立，PAYH，LIBRARY 等环境变量管理较为麻烦，因此 brew 又维护了一个映射关系，
将所有文件软链接到 /usr/local 的 bin, etc, lib, var 等文件夹之中。
正是这个道理 mysql 安装的数据库的位置可能就位于 /usr/local/var/db 之中。

另外，Mac 的 GUI软件（即 Cocoa 软件）全部是按照软件包的形式发放，没有分散的文件。
brew 也可以安装 chrome 等这类 GUI 软件，此时位置全部放置在默认位置 /Applications 之中。