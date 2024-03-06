---
layout: default
title: adb
parent: Java

---

## 1. 连接与退出

设置目标设备以监听端口 5555 上的 TCP/IP 连接

> adb 127.0.0.1 5555

> adb connect 127.0.0.1:5555

关闭当前服务，会把所有连接断开

> adb kill-server

## 2. 显示所有连接的设置

adb devices -l

## 3. 文件交互

adb -s 0.0.0.0:6520 install helloWorld.apk

adb install path_to_apk
adb install -t path_to_apk

adb pull remote local

文件上传

adb push local remote

adb push myfile.txt /sdcard/myfile.txt

## 4. shell

可以指定的具体的设置，进行shell交互，MROVLJAUJZJZPZKB就是某个设备的
序列号
> adb [-d | -e | -s serial_number] shell
>
> adb -s MROVLJAUJZJZPZKB shell ls /system/bin

### 4.1. am activity manage

### 4.2. pm package manage