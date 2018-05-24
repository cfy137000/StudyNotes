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
配置Groovy环境的时候主要分成2步:
1. 下载Groovy
2. 解压并配置环境变量

#### 下载
打开groovy的[下载页面](http://groovy-lang.org/download.html),我们会发现有很多版本,以目前来说(2018.5.24)它提供了:
- Groovy2.6: Bleeding Edge Vesion
- Groovy2.5: upcoming version
- Groovy2.4: latest stable version

> Bleeding Edge Vesion: 这个词不是很好翻译,它指的是那种最新的技术,但是需要承担一定的风险,并且使用者为了追求这种技术的新,也乐于承担这种风险的版本,而钢铁侠不是也有一套血边装甲嘛,好像用的也是bleeding edge这个词,不知道是否也有这种意思

![图文无关](http://47.93.60.69:88/img/pics/44964259D2CE4B8D8DA7F833C879BD3B.png?x-oss-process=style/CfyInfo)


那么我们选择下载2.4这个版本,它是最新的稳定版
![](http://47.93.60.69:88/img/pics/C16DFBCF6D22429CBEB3B0B19E15EE4A.png?x-oss-process=style/CfyInfo)

选择下载这个SDK bundle,或者这个页面最上方大大下载按钮,对应的也是最新稳定版的下载链接:   
![](http://47.93.60.69:88/img/pics/940C7436805C48DDB5F63D938358B362.png?x-oss-process=style/CfyInfo)

在下载的时候要注意,groovy的版本和jdk的版本是需要对应的

![](http://47.93.60.69:88/img/pics/7FE16ABA73F14A08964CDE97F1B8C281.png?x-oss-process=style/CfyInfo)

这个indy和non-indy的意思是 需不需要支持Java的InvokeDynamic指令,这是java7中新增的一条字节码指令,如果我们要使用的groovy使用这条指令进行编译的话那么至少需要1.7以上的java版本,而如果不使用该条指令的话,2.4其实也支持到1.6的jvm的,其实在我们写groovy代码时影响不会很大,而且默认使用的也是groovy non-indy的版本,主要影响的是编译的过程,这里建议使用java8以上版本,毕竟java8也出了好长时间了,而且是最新的长期支持版本

#### 解压并配置环境变量
下载好了之后,就可以解压到想要的地方了

![解压](http://47.93.60.69:88/img/pics/62F603B747D64070B734EB4E875BC9BD.png?x-oss-process=style/CfyInfo)


解压好了之后,和Java一样我们需要配置环境变量:

```sh
subl ~/.zshrc
```

```sh
# groovy
export GROOVY_HOME=/home/chen/Programs/groovy-2.4.15
export PATH=${GROOVY_HOME}/bin:$PATH
```

其实就是把bin文件夹中配置进path中,配置完成后重启终端,然后输入:

```sh
groovy -v
```

看到    
![](http://47.93.60.69:88/img/pics/06D6CB57CB6F417B8E1B07305558AF5F.png?x-oss-process=style/CfyInfo)    
就代表我们已经成功配置了groovy的环境变量

## 创建项目
我使用的idea进行创建groovy项目,idea的免费版也是支持groovy项目创建的,所以还没有使用idea的小伙伴可以尝试一波

![](http://47.93.60.69:88/img/pics/06B46695B9E04341AC08E8DDB6C3B1DE.png?x-oss-process=style/CfyInfo)

在创建项目的时候,我们注意要选择groovy就可以了,图中`2`的部分 如果idea没能正确识别你的groovy和java的安装路径的话,需要手动选一下,之后就一路next,正常的创建项目就可以了

## 基础语法
