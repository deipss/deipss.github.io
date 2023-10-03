---
layout: default
title: mvn
parent: Command
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

# 8. 源码下载

```shell
mvn dependency:sources
mvn dependency:resolve -Dclassifier=javadoc
mvn dependency:sources dependency:resolve -Dclassifier=javadoc
```

# 9. mvn 仓库优先级

```shell
本地仓库 > 私服 （profile）> 远程仓库（repository）和 镜像 （mirror） > 中央仓库 （central）
```

# 10. nexus

nexus里可以配置3种类型的仓库，分别是proxy、hosted、group 。

- proxy是远程仓库的代理。比如说在nexus中配置了一个central
  repository的proxy，当用户向这个proxy请求一个artifact，这个proxy就会先在本地查找，如果找不到的话，就会从远程仓库下载，然后返回给用户，相当于起到一个中转的作用。
- hosted是宿主仓库，用户可以把自己的artifact、proxy下载不到的artifact，deploy到hosted中。
-
group是仓库组，目的是将上述多个仓库聚合，对用户暴露统一的地址，这样用户就不需要在pom中配置多个地址，只要统一配置group的地址就可以了 。

# 11. plugins

https://maven.apache.org/plugins/index.html

## 11.1. basic

- clean
- compiler
- deploy
- install
- resources
- failsafe which ensure isolated classloader to run Junit test
- surefire which ensure isolated classloader to run Junit Integrated test
- verifier

## 11.2. tools

- findbugs
- checkstyle
- spring-boot-maven-plugin
- flatten-maven-plugin