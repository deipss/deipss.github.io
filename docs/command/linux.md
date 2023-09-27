---
layout: default
title: linux
parent: Command
nav_order: 5
---

# 1. ubuntu show version

```shell
lsb_release -a
lscpu
```

# 2. 开机启动
```shell
systemctl enable nginx.service       
systemctl enable supervisord

systemctl disable nginx.service
systemctl disable supervisord

systemctl is-enabled nginx
systemctl is-enabled supervisord
```

# 3. 日志查看

- [https://cloud.tencent.com/developer/article/1579977](https://cloud.tencent.com/developer/article/1579977)

## 3.1. tail

```bash
命令格式: tail[必要参数][选择参数][文件]
-f 循环读取
-q 不显示处理信息
-v 显示详细的处理信息
-c<数目> 显示的字节数
-n<行数> 显示行数
-q, --quiet, --silent 从不输出给出文件名的首部
-s, --sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒

tail -fn 1000 test.log | grep '关键字'
```

## 3.2. head

```bash
head -n  10  test.log   查询日志文件中的头10行日志;
head -n -10  test.log   查询日志文件除了最后10行的其他所有日志;
```

## 3.3. less

less命令在查询日志时，less与more类似，使用less可以随意浏览文件，而more仅能向前移动，不能向后移动，而且 less 在查看之前不会加载整个文件。

```bash
less -mN log2013.log 
-g 只标志最后搜索的关键词
-m 显示类似more命令的百分比
-N 显示每行的行号
/字符串：向下搜索"字符串"的功能
?字符串：向上搜索"字符串"的功能
n：重复前一个搜索（与 / 或 ? 有关）
N：反向重复前一个搜索（与 / 或 ? 有关）
b 向上翻一页
d 向后翻半页
h 显示帮助界面
Q 退出less 命令
u 向前滚动半页
y 向前滚动一行
空格键 滚动一页
回车键 滚动一行
G 跳到末行
```

## 3.4. more

more命令是一个基于vi编辑器文本过滤器，它以全屏幕的方式按页显示文本文件的内容，支持vi中的关键字定位操作。
more内置了若干快捷键，常用的有H（获得帮助信息），

- Enter（向下翻滚一行），
- 空格（向下滚动一屏），
- Q（退出命令）。
- more命令从前向后读取文件，因此在启动时就加载整个文件。

该命令一次显示一屏文本，
满屏后停下来，并且在屏幕的底部出现一个提示信息，给出至今己显示的该文件的百分比：

## 3.5. cat

cat（英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上。

```shell
#把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：
cat -n textfile1 > textfile2
#把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：
cat -b textfile1 textfile2 >> textfile3
```

## 3.6. grep

Linux grep (global regular expression) 命令用于查找文件里符合条件的字符串或正则表达式。

```shell
grep "字符串" 文件名 | grep "字符串"
grep "^字符串" 文件名


从文件内容查找匹配指定字符串的行： $ grep "被查找的字符串" 文件名

从文件内容查找与正则表达式匹配的行： $ grep –e “正则表达式” 文件名

查找时不区分大小写： $ grep –i "被查找的字符串" 文件名

查找匹配的行数： $ grep -c "被查找的字符串" 文件名

从文件内容查找不匹配指定字符串的行：  $ grep –v "被查找的字符串" 文件名

从根目录开始查找所有扩展名为.log的文本文件，并找出包含”ERROR”的行 find / -type f -name "*.log" | xargs grep "ERROR"

倒序输出搜索到的内容
grep "ERROR" app-repeater-receiver-2022-05-06-1.log | grep "saveRecord" | sort -k2 -n -r -t:
```

# 4. 文件查找

- http://www.ruanyifeng.com/blog/2009/10/5_ways_to_search_for_files_using_the_terminal.html

## 4.1. find

```bash
find . -name 'my*'
find . -name 'my*' -ls
# 查找近10分钟修改过的文件
find -type f -mmin 10
find / -name "my*" 从根目录开始查找
```

## 4.2. locate

类似于find -name，但是效率比find高

```bash
locate /etc/my
```

## 4.3. whereis

只查找bin文件，即一些命令

```bash
whereis java
where grep
```

## 4.4. which

从PATH中查找

```bash
which java
which grep
```

# 5. 网络信息查询

## 5.1. ifconfig

```shell
ifconfig -address
```

## 5.2. telnet ip port

检查某个服务的port是否启动

## 5.3. tcpdump

```shell
yum install tcpdump
-w write写入某个文件 
tcpdump -w package.cap  
```

## 5.4. netstat

```shell
netstat -s | egrep "listen|LISTEN" 等同  netstat -s | grep -i "listen"
netstat -at 查询所有tcp的连接，可用于服务启动后的端口查看，是否启动了kafka，dubbo等
netstat -nap | grep port 将会显示使用该端口的应用程序的进程 id
netstat -g 将会显示该主机订阅的所有多播网络。
```

## 5.5. traceroute

```shell
traceroute baidu.com
```

## 5.6. ss

```bash
ss -int
```

## 5.7. ubuntu网络防火端设置信息

```shell
sudo ufw allow 22/tcp
sudo ufw allow 21/tcp
sudo ufw allow 23/tcp
sudo ufw allow 80/tcp
sudo ufw allow from 59.75.133.222
sudo ufw enable|disable


sudo ufw delete allow 22/tcp
sudo ufw delete allow 21/tcp
sudo ufw delete allow 23/tcp
sudo ufw delete allow 80/tcp
```

## 5.8. 内网穿透

# 6. 硬盘使用

## 6.1. df

df（英文全称：disk free）：列出文件系统的整体磁盘使用量

```shell
df -hf
df -h /etc
```

## 6.2. du

- h或--human-readable 以K，M，G为单位，提高信息的可读性。
- -s或--summarize 仅显示总计。

```shell
du * sh 
```

# 7. 系统环境

- https://www.cnblogs.com/youyoui/p/10680329.html

## 7.1. 修改环建变量

- Linux环境变量配置方法一：export PATH

```shell
export PATH=/home/uusama/mysql/bin:$PATH

# 或者把PATH放在前面
export PATH=$PATH:/home/uusama/mysql/bin
注意事项：
生效时间：立即生效
生效期限：当前终端有效，窗口关闭后无效
生效范围：仅对当前用户有效
配置的环境变量中不要忘了加上原来的配置，即$PATH部分，避免覆盖原来配置

```

- Linux环境变量配置方法二：vim ~/.bashrc

```shell
vim ~/.bashrc

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
注意事项：

生效时间：使用相同的用户打开新的终端时生效，或者手动source ~/.bashrc生效
生效期限：永久有效
生效范围：仅对当前用户有效
如果有后续的环境变量加载文件覆盖了PATH定义，则可能不生效

```

- Linux环境变量配置方法三：vim ~/.bash_profile

```shell
和修改~/.bashrc文件类似，也是要在文件最后加上新的路径即可：

vim ~/.bash_profile

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
注意事项：

生效时间：使用相同的用户打开新的终端时生效，或者手动source ~/.bash_profile生效
生效期限：永久有效
生效范围：仅对当前用户有效
如果没有~/.bash_profile文件，则可以编辑~/.profile文件或者新建一个
```

- Linux环境变量配置方法四：vim /etc/bashrc

```shell
该方法是修改系统配置，需要管理员权限（如root）或者对该文件的写入权限：

# 如果/etc/bashrc文件不可编辑，需要修改为可编辑
chmod -v u+w /etc/bashrc

vim /etc/bashrc

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
注意事项：

生效时间：新开终端生效，或者手动source /etc/bashrc生效
生效期限：永久有效
生效范围：对所有用户有效

```

- Linux环境变量配置方法五：vim /etc/profile

```shell
该方法修改系统配置，需要管理员权限或者对该文件的写入权限，和vim /etc/bashrc类似：

# 如果/etc/profile文件不可编辑，需要修改为可编辑
chmod -v u+w /etc/profile

vim /etc/profile

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
注意事项：

生效时间：新开终端生效，或者手动source /etc/profile生效
生效期限：永久有效
生效范围：对所有用户有效
```

## 7.2. 环境变量分类

环境变量可以简单的分成用户自定义的环境变量以及系统级别的环境变量。

- 用户级别环境变量定义文件：~/.bashrc、~/.profile（部分系统为：~/.bash_profile）
- 系统级别环境变量定义文件：/etc/bashrc、/etc/profile(部分系统为：/etc/bash_profile）、/etc/environment

Linux加载环境变量的顺序如下：

```text
/etc/environment
/etc/profile
/etc/bash.bashrc
/etc/profile.d/test.sh
~/.profile
~/.bashrc
```

## 7.3. 查看环境变量

- env 查看当前主机的所有环境环境
- export命令显示当前系统定义的所有环境变量
- echo $PATH命令输出当前的PATH环境变量的值

```shell
env | grep -i 'env' 在环境变量中查找包括env字符的行
```

# 8. 进程

## 8.1. ps

```shell

查看某个关键字的进程
ps aux | grep java  
ps -ef | grep  <关键字>

ps -auf
ps -L <pid>

```

## 8.2. lsof (list open files)列出当前系统打开文件

```shell
lsof -i:<端口号>
lsof -i
```

## 8.3. kill

- kill -9
- kill -15

- https://www.runoob.com/linux/linux-comm-kill.html 菜鸟

## 8.4. dmseg

Linux dmesg（英文全称：display message）命令用于显示开机信息。

- dmseg -t > dmseg1.log

# 9. 其他

## 9.1. nohup

```shell
nohup /root/runoob.sh > runoob.log 2>&1 &
# 2>&1 解释：
# 将标准错误 2 重定向到标准输出 &1 ，标准输出 &1 再被重定向输入到 runoob.log 文件中。
# 0 – stdin (standard input，标准输入)
# 1 – stdout (standard output，标准输出)
# 2 – stderr (standard error，标准错误输出)
```