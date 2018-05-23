# Gradle01

## gradle概述

### groovy和gradle
groovy是一种基于JVM的敏捷开发语言,它结合了Python,Ruby和Smalltalk的许多特性,同时它的代码能够和Java代码很好的结合,并且由于它可以运行在JVM上,所以Groovy可以直接使用其他的Java的jar包

- Groovy语言=Java语言的扩展+众多脚本语言的语法。运行在JVM虚拟机上
- Gradle项目构框架使用groovy语言实现。基于Gradle框架为我们实现了一些项目构件框架


## 开发环境搭建
groovy是需要运行在java虚拟机上的,所以需要安装java环境

### java环境
到java的官网下载jdk [http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

![](http://47.93.60.69:88/img/pics/7FA16126033545D4AEB3A0533B2178A9.png?x-oss-process=style/CfyInfo)

然后解压下载好的压缩包,到硬盘中的任意位置

![](http://47.93.60.69:88/img/pics/2BE5B8FD88AA4E1E8CFDC96FA71D5673.png?x-oss-process=style/CfyInfo)

然后配置环境变量,在终端中输入

```sh
subl ~/.zshrc
```

打开环境变量的配置,并添加java的配置

> 注: 我安装了 sublim text3,所以使用subl命令,如果没有,请使用vim命令,并且我的终端替换成了zsh,否则环境变量文件是.bash_profile

```sh
# JAVA
export JAVA_HOME=/home/chen/Programs/jdk1.8.0_171  
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
```

重启终端,在终端中输入

```sh
java -version
```

![](http://47.93.60.69:88/img/pics/F8B7476E8CBE49C5A65E8DB2B54BD88B.png?x-oss-process=style/CfyInfo)

看到显示java的相关信息了,则表示java环境变量配置成功

### Groovy环境

## 基础语法