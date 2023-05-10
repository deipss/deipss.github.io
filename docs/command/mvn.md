---
layout: default
title: mvn
parent: command
nav_order: 6
---

# 1. 依赖树打印

```shell
mvn dependency:tree > mvnTree.txt
```

# 2. 设置版本

```shell
mvn versions:set -D newVersion=1.5.0-SNAPSHOT
```

# 3. mvn -D -P -U

- P代表（Profiles配置文件）

```shell

<profiles>
   <profile>
     <id>prod</id>
     ...
   </profile>
   <profile>
     <id>test</id>
     ...
   </profile>
  </profiles>
打包时执行mvn clean package -P test将触发test环境的profile配置 

```

- -D代表（Properties属性）
- pl 表示只处理指定的module
- am 表示处理-pl指定module关联的其他module

```shell

<properties>
  <attr>defaultattr</attr>
</properties>
执行 mvn -Dattr=newattr clean package，则pom.xml内attr的实际值将被替换成newattr

```

- -U 代表强制更新（update）

# 4. 跳过测试

- mvn -Dmaven.test.skip=true clean package ，跳过测试包下的程序

# 5. 部署到仓库

```shell

mvn deploy:deploy-file -DgroupId=<group-id> \
-DartifactId=<artifact-id> \
-Dversion=<version> \
-Dpackaging=<type-of-packaging> \
-Dfile=<path-to-file> \
-DrepositoryId=<id-to-map-on-server-section-of-settings.xml> \
-Durl=<url-of-the-repository-to-deploy>

# 打jar包 windows ^ 标记换行
mvn deploy:deploy-file ^
-DgroupId=com.alibaba.jvm.sandbox  ^
-DartifactId=repeater-console-service  ^
-Dversion=1.0.0-SNAPSHOT  ^
-Dpackaging=jar  ^
-Dfile=E:\temp2\repeater-console-service\1.0.0-SNAPSHOT\repeater-console-service-1.0.0-SNAPSHOT.jar ^
-DrepositoryId=maven-snapshots  ^
-Durl=http://nexus.xsyxsc.com/repository/maven-snapshots

# 打pom包
mvn deploy:deploy-file 
-DgroupId=com.alibaba.jvm.sandbox 
-DartifactId=repeater-console 
-Dversion=1.0.0-SNAPSHOT 
-Dpackaging=pom 
-Dfile=C:\Users\PC\IdeaProjects\test\jvm-sandbox-repeater\repeater-console\pom.xml 
-DrepositoryId=maven-snapshots 
-Durl=http://nexus.xsyxsc.com/repository/maven-snapshots





<!-- 仅在dev的profile中，开启SNAPSHOT的仓库 -->
    <repositories>
        <repository>
            <id>nexus-snapshots</id>
            <name>Nexus Local Repository</name>
            <url>http://nexus.xsyxsc.com/repository/maven-snapshots/</url>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </snapshots>
        </repository>
    </repositories>
```

# 6. 安装在本地

```shell
mvn install 会将包安装在本地的仓库
mvn clean compile install deploy  -Dmaven.test.skip=true
mvn clean package  '-Dmaven.test.skip=true'

```

# 7. mvn配置文件

```text
<package></package>标签 可以有pom或是jar,默认是jar,在有父子继续关系时，一般父pom文件使用pom包
```