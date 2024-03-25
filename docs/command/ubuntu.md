---
layout: default
title: ubuntu
parent: Command

---

# 1. ubuntu show version

```shell
lsb_release -a
lscpu
```

# 2. ubuntu systemctl
```shell
# start with reboot
systemctl enable nginx.service       
systemctl enable supervisord

systemctl disable nginx.service
systemctl disable supervisord

systemctl is-enabled nginx
systemctl is-enabled supervisord
# show all
systemctl list-unit-files --type service -all

# start
systemctl start <service-name>
systemctl stop <service-name>
systemctl restart <service-name>
systemctl status <service-name>


```

# 3. ubuntu services

```shell
service --status-all
service <service-name> start
service <service-name> stop
service <service-name> restart
service <service-name> status

```

# 4. ubuntu install jdk
```shell
# 安装jdk8
sudo apt install openjdk-8-jdk
whereis java

# 配置jre等包,不配置的话，运行的java程序，会找不到CLASSPATH下的包，导致一些功能不能正常使用，比如远程debug
vim ~/.profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```
 

# 5. ubuntu网络防火端设置信息

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


# 6. install v2ray
- https://junz.org/post/v2_in_linux/
 

# 7. 系统环境
  
使用.profile文件作为环境变量的配置文件