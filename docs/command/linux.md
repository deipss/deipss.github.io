---
layout: default
title: linux
parent: Command
nav_order: 5
---

# 1. ubuntu网络防火端设置信息

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

# 2. 日志查看

- [https://cloud.tencent.com/developer/article/1579977](https://cloud.tencent.com/developer/article/1579977)

## 2.1. tail

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

## 2.2. head

```bash
head -n  10  test.log   查询日志文件中的头10行日志;
head -n -10  test.log   查询日志文件除了最后10行的其他所有日志;
```

## 2.3. less

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

## 2.4. more

more命令是一个基于vi编辑器文本过滤器，它以全屏幕的方式按页显示文本文件的内容，支持vi中的关键字定位操作。more名单中内置了若干快捷键，常用的有H（获得帮助信息），Enter（向下翻滚一行），空格（向下滚动一屏），Q（退出命令）。more命令从前向后读取文件，因此在启动时就加载整个文件。
该命令一次显示一屏文本，满屏后停下来，并且在屏幕的底部出现一个提示信息，给出至今己显示的该文件的百分比：

## 2.5. cat

## 2.6. grep

```bash
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

# 3. 文件查找

- http://www.ruanyifeng.com/blog/2009/10/5_ways_to_search_for_files_using_the_terminal.html

## 3.1. find

```bash
find . -name 'my*'
find . -name 'my*' -ls
# 查找近10分钟修改过的文件
find -type f -mmin 10
find / -name "my*" 从根目录开始查找
```

## 3.2. locate

类似于find -name，但是效率比find高

```bash
locate /etc/my
```

## 3.3. whereis

只查找bin文件，即一些命令

```bash
whereis java
where grep
```

## 3.4. which

从PATH中查找

```bash
which java
which grep
```

# 4. 网络信息查询

## 4.1. ipconfig

```shell
ifconfig -address
```

## 4.2. telnet ip port

检查某个服务的port是否启动

## 4.4. tcpdump

```shell
yum install tcpdump
-w write写入某个文件 
tcpdump -w package.cap  
```

## 4.5. netstat

```shell
netstat -s | egrep "listen|LISTEN" 等同  netstat -s | grep -i "listen"
netstat -at 查询所有tcp的连接，可用于服务启动后的端口查看，是否启动了kafka，dubbo等
netstat -nap | grep port 将会显示使用该端口的应用程序的进程 id
netstat -g 将会显示该主机订阅的所有多播网络。
```

## 4.6. ps -efl

## 4.7. ss

```bash
ss -int
```

# 5. 硬盘使用

## 5.1. df -hl

# 6. 系统环境

## 6.1. env 查看当前主机的所有环境环境

```shell
env | grep -i 'env' 在环境变量中查找包括env字符的行
```

# 7. 其他

## 7.1. nohup

```shell
nohup /root/runoob.sh > runoob.log 2>&1 &
# 2>&1 解释：
# 将标准错误 2 重定向到标准输出 &1 ，标准输出 &1 再被重定向输入到 runoob.log 文件中。
# 0 – stdin (standard input，标准输入)
# 1 – stdout (standard output，标准输出)
# 2 – stderr (standard error，标准错误输出)
```