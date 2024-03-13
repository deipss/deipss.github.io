---
layout: default
title: adb
parent: Mobile

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

```shell
 adb [-d | -e | -s serial_number] shell
 adb -s MROVLJAUJZJZPZKB shell ls /system/bin
```

### 4.1. am activity manage

adb shell am

### 4.2. pm package manage

adb shell pm

### 4.3. dumpsys

下载所有的服务

adb shell dumpsys -l

```shell
or:
       dumpsys [-t TIMEOUT] [--priority LEVEL] [--pid] [--thread] [--help | -l | --skip SERVICES | SERVICE [ARGS]]
         --help: shows this help
         -l: only list services, do not dump them
         -t TIMEOUT_SEC: TIMEOUT to use in seconds instead of default 10 seconds
         -T TIMEOUT_MS: TIMEOUT to use in milliseconds instead of default 10 seconds
         --pid: dump PID instead of usual dump
         --thread: dump thread usage instead of usual dump
         --proto: filter services that support dumping data in proto format. Dumps
               will be in proto format.
         --priority LEVEL: filter services based on specified priority
               LEVEL must be one of CRITICAL | HIGH | NORMAL
         --skip SERVICES: dumps all services but SERVICES (comma-separated list)
         SERVICE [ARGS]: dumps only service SERVICE, optionally passing ARGS to it

```
国
## 5. logcat

adb logcat --help

  