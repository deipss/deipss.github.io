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
```shell

cd "$(brew --repo)"
git config -l 先查看下当前git的配置
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

```
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
JAVA_11_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.1.jdk/Contents/Home/
# JDK 8
JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home
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


# 5. CPU
用来操作计算单元的控制单元的指令集合就是CPU指令集，有X86、RAM、MIPS、IA64，可以分成复杂指令集和
精简指令集两类，CISC和RISC。
PC端主要是X86，移动端主是要RAM。
```shell
uname -pa

处理器体系结构的类型	ARM	            x86/x64
架构开发公司	        ARM	            Intel /AMD
处理器制造公司	        Intel,apple	    Intel /AMD
指令集的架构	        RISC	        CISC
主要使用的用途	        智能手机，平板电脑	电脑，PC服务器
32位/64位	        两者都有	        x86:32位/x64:64位
字节存储次序	        bi-endian	    Little endian
```