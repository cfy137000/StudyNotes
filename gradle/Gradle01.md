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

### HelloWorld

创建好项目之后,我们就可以来写groovy代码了,之前说过groovy是完全兼容Java的,而兼容到什么程度呢,首先我们先按Java代码的语法来写一个Groovy的HelloWorld

创建一个Groovy的类      
![](http://47.93.60.69:88/img/pics/62DE5F39D4084AC183BDE67B208A1F63.png?x-oss-process=style/CfyInfo)

我们叫它HelloGroovy

```groovy
class HelloGroovy {

    public static void main(String[] args) {
        System.out.println("Hello Groovy");
    }
}
```

运行一下:

![](http://47.93.60.69:88/img/pics/41C7D1EFE5854BAFA2EEE2F8A852334E.png?x-oss-process=style/CfyInfo)

可以看到我们可以正常输出了,观察代码发现,这不就完全是Java代码嘛,是的在Groovy中,你甚至可以写Java代码,那么groovy又在java代码的基础上可以简化成什么样子呢?      

我们知道,java要求我们:
- 所有的代码必须写在一个类中
- 程序需要有一个主方法来运行
- 每一行语句都以分号结束

但是我们的groovy就要灵活很多了,这些Java中的强制性要求,在groovy中都不存在,修改groovy代码如下:

```groovy
System.out.println("Hello Groovy")
```

![](http://47.93.60.69:88/img/pics/F8FCD7BE1A6649A1B4111CBB7F8B0390.png?x-oss-process=style/CfyInfo)


是的,就这一句,groovy是个脚本语言,它不需要类和方法,程序可以直接由上向下一行一行的执行,并且每一行的后面也不需要分号,我们还可以简化这输出语句:

```groovy
println "Hello Groovy"
println("Hello")
```

直接写println即可,并且在groovy中 允许在 `顶级表达式`中省略参数列表的小括号,就是你直接调用方法的参数,可以省略,要是方法的参数还是一个方法,就不能省略小括号,例如:

```groovy
println add(3, 4)
println(add(3, 4))

def add(a, b) {
    return a + b
}
```

这个add的小括号就不能省略,这个后面到方法的时候再说,总之可以发现使用groovy来写java代码,要比纯Java代码简洁很多,恩,语法其实有点类似与Python呢,而Groovy的代码,也需要编译,编译成的文件也是.class文件,这就是为什么Groovy代码能够在JVM上运行,我们直接用idea打开HelloGroovy.class文件,idea会自动把编译后的字节码给我们反编译回来

```java
import groovy.lang.Binding;
import groovy.lang.Script;
import org.codehaus.groovy.runtime.InvokerHelper;
import org.codehaus.groovy.runtime.callsite.CallSite;

public class HelloGroovy extends Script {
    public HelloGroovy() {
        CallSite[] var1 = $getCallSiteArray();
        super();
    }

    public HelloGroovy(Binding context) {
        CallSite[] var2 = $getCallSiteArray();
        super(context);
    }

    public static void main(String... args) {
        CallSite[] var1 = $getCallSiteArray();
        var1[0].call(InvokerHelper.class, HelloGroovy.class, args);
    }

    public Object run() {
        CallSite[] var1 = $getCallSiteArray();
        return var1[1].call(var1[2].callGetProperty(System.class), "Hello Groovy");
    }
}

```

可以看到所有我们省略的东西,Groovy在编译成字节码的时候,编译器都给我们自动加回来了~

### 变量

#### 数据类型

在groovy中可以使用所有Java的数据类型,我们知道在Java中数据类型分为基本数据类型,和引用类型,而在groovy中则没有基本数据类型了,在groovy中,所有的基本数据类型都会在编译的时候转换成其包装类型:

```groovy
int x = 10
println x
println x.class
```

![](http://47.93.60.69:88/img/pics/63D55ECE0F604F7584208ED76689AC6D.png?x-oss-process=style/CfyInfo)

可以看到我们定义一个int类型的变量,在输出这个变量的类型的时候,发现它的类型是Integer,即它自动被转换成了引用类型

#### 定义变量

我们的Java是一门强类型语言,所有的变量的在定义的时候就需要声明好它的数据类型,例如:

```java
public void fun(){
    // JAVA代码
    String name = "张三";
    int age = 18;
}
```

但是Groovy并不是,它有类型推断机制,就比如我们上面Java代码,到了groovy中,除了可以写成和Java一模一样的以外,我们还可以这样写:

```groovy
def name = "张三"
def age = 18

println name
println age
println name.class
println age.class
```

![运行结果](http://47.93.60.69:88/img/pics/05145C83D41346B28412D94E26C8AF74.png?x-oss-process=style/CfyInfo)

可以看到,无论是String类型,还是int类型,我们都使用的是def来进行定义变量的,而在输出他们的数据类型的时候,Groovy也知道name就是String类型,age就是Integer类型,这就是类型推断,因为我们在定义变量的时候,直接为数据类型进行赋值了,那么编译器就会根据等号右面的值的数据类型,来决定等号左边变量的数据类型

> 类型推论、类型推断、或隐含类型，是指编程语言在编译期中能够自动推导出值的数据类型的能力，它是一些强静态类型语言的特性。一般而言，函数式编程语言也具有此特性。自动推断类型的能力让很多编程任务变得容易，让程序员可以忽略类型标注的同时仍然允许类型检查

其实Java中的类型推断机制也是逐步被加强的,例如在Java6中你需要这样定义一个String泛型的集合:

```java
// java6
List<String> strings = new ArrayList<String>();
```

而到了Java7中我们可以这样写:

```java
// java7
List<String> strings = new ArrayList<>();
```

可以看到编译器已经越来越智能了,不需要我们什么都写出来了,而java8中的lambda表达式也是这种类型推测机制更加强大的体现

那么何时定义变量的时候使用强类型定义,何时定义变量的时候使用def呢,总结下来有2点:
1. 如果变量只用于自己这个类而不会使用于其他模块,则使用def
2. 如果用于其他类的话 那么则使用强类型定义

其实无论是强类型定义还是弱类型定义,都是为了我们写程序更加的方便,易懂,那么如果我们这个模块要给外部使用的时候,如果我们也使用def的话,就会造成调用方的迷惑,不知道传给我们什么类型的参数好了

##### 动态类型
Java是一门静态类型的语言,即当数据的类型一旦确定下来之后,就不能够在进行更改了,否则编译都过不去:

```java
public class Main {
    public static void main(String[] args) {
        String s = "张三";
        s = 18;
    }
}
```

![](http://47.93.60.69:88/img/pics/D587090B33E44F2AA72552C25F1F7E04.png?x-oss-process=style/CfyInfo)        

那么换到我们的groovy中呢,我们在定义变量的时候可以不指定类型,而去使用def,那么我们可以写出如下代码:

```groovy
def a = "张三"
println a.class
println a
a = 18
println a.class
println a
a = 3.14
println a.class
println a
```

![](http://47.93.60.69:88/img/pics/32543F24A25E4998B4F2F58CA4ABC173.png?x-oss-process=style/CfyInfo)


可以看到在groovy中我们就定义了一个a变量,而这个a变量可以根据运行时值的不同,而动态的改变它的数据类型,我们的Groovy是动态类型的语言,在写起来要比Java灵活很多,那么如果我们使用传统的强类型定义方式去定义一个变量,变量的类型是不是就不能动态的改变了呢,看代码

```groovy
String a = "张三"
println a.class
println a
a = 18
println a.class
println a
```

![](http://47.93.60.69:88/img/pics/5AE9310D3C76458698D67C919C8470E3.png?x-oss-process=style/CfyInfo)

可以看到,强类型定义的方式,我们的数据类型就被固定上了,不能再去改变了,并且如果两个数据类型没办法兼容的话,程序还会报错

```groovy
int a = 3
a = '张三'
```

![](http://47.93.60.69:88/img/pics/5899FB5681354DEEABBB425FB462BA97.png?x-oss-process=style/CfyInfo)


## String类型

String类型是Java中非常常用的一种数据类型,甚至可以说是最常用的了,Groovy对Java中的String类型做了一些扩展,并把这些扩展封装进了GString(groovy.long.GString)中,就是说在Groovy中的字符串,对应两个类:String和GString,String类和Java中的String没有任何区别,主要看的是Groovy中特殊的GString

### 定义String

在Java中定义String是使用双引号:

```java
String a = "Hello World";
```

但是在Groovy中就不只只有这一种方法了,在Groovy中可以使用单引号,双引号和三引号(三个单引号:''')来定义一个String

```Groovy
def a = 'Hello'
def b = "Hello"
def c = '''Hello'''

println a.class
println b.class
println c.class
```

![](http://47.93.60.69:88/img/pics/B35A7D49DC9647C7B53E5B708CDD248C.png?x-oss-process=style/CfyInfo)

可以看到这三种方式都可以定义字符串,那么他们的区别是什么呢?

#### 单引号

在groovy中使用单引号定义的字符串就是Java中最最普通的字符串,不能插值

#### 双引号
在Groovy中如果双引号定义的字符串中没有插值表达式,那么双引号字符串就是普通的String类,如果有差值表达式的存在,则是GString的实例,插值表达式在下面会详细说明

#### 三引号
三引号是多行字符串,使用起来和Python是一样的,可以直接在里面写换行,而不需要使用类似于`\n`这种的符号来表示换行

```groovy
def s = '''锄禾日当午
汗滴禾下土
谁知盘中餐
粒粒皆辛苦
'''

println s
```

![](http://47.93.60.69:88/img/pics/6302A4F4BE634AB387643DD2D6D343A2.png?x-oss-process=style/CfyInfo)

需要注意的是,这个字符串的每一次换行,其实都是给你自动的添加一个\n的

```groovy
def s = '''锄禾日当午
汗滴禾下土
谁知盘中餐
粒粒皆辛苦
'''

println s.contains('\n')
println s.size()
```

![](http://47.93.60.69:88/img/pics/844A3039A3734BA9BC2704930A993E43.png?x-oss-process=style/CfyInfo)

可以看到字符串中是包含`\n`这个字符的,并且字符串的长度是24,20个字加上4个`\n`,那么如果我不想换行呢,可以使用反斜杠来取消换行:

```groovy
def s = '''\
锄禾日当午,\
汗滴禾下土
谁知盘中餐,\
粒粒皆辛苦\
'''

println s
```

![](http://47.93.60.69:88/img/pics/92607B88A82745F7A12EF99147AA0474.png?x-oss-process=style/CfyInfo)


### 插值表达式
当使用双引号进行表示字符串时,我们可以使用字符串插值,就是用固定格式的字符串来进行占位,实际生成的时候,占位表达式会被动态的替换成响应的字符串

在groovy中使用`${表达式}`的形式来进行占位:

```groovy
def youName = 'cfy'
def sayHello = "Hello: ${youName}"
println sayHello
```

![](http://47.93.60.69:88/img/pics/92F1A7B3871843EAB23BE2207F6ABEA2.png?x-oss-process=style/CfyInfo)

