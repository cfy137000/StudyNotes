@[TOC](使用AndroidStudio查看并调试Android源码)

# 使用AndroidStudio查看并调试Android源码
无论是在工作中,还是我们自己学习Android,总会用到Android的源码,没有趁手的开发工具,我们是无法进行调试,编写代码的,而AndroidStudio就可以做到编译源码并调试源码

## 0.基础环境

### 1. 操作系统
首先我们需要一个Linux的操作系统,直接在Windows下是没法编译的,在这里,我使用的是Deepin,而Ubuntu的操作应该与我一样,如果你只有一个Windows电脑,强烈推荐使用Docker来完成,不要使用虚拟机.

### 2. Java
Java使用1.8即可,本文编译的是Android-P,而根据Android版本的不同,需要的Java环境可能也略有区别
![](http://47.93.60.69:88/img/pics/2018/6E4A4DF1B9D142FC87DBA3F53E007145.png?x-oss-process=style/CfyInfo)

### 3. Android源码
关于源码的下载,不是本文的重点,不会赘述,建议使用清华源进行下载,关于如何下载,网址写的还是比较详细的:[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/)

## 1. 编译源码
在将工程导入到AndroidStudio之前,我们最好先编译一下整个Base代码,这样一来可以保证我们的代码和开发环境没有什么问题,二来可以通过编译来生成R文件

### 初始化编译环境
命令:

```sh
source build/envsetup.sh 
```

这里需要注意一点, **确保你的终端是bash或者zsh**,因为Android的编译脚本只保证兼容这两个,如果是fish什么的就要手动切换一下了,并且AndroidP才支持的zsh,在AndroidO上还支持从bash,我就在编译AndroidO的时候,由于使用了zsh而遇到了一些坑

#### AndroidP的终端检查源码

```sh
function validate_current_shell() {
    local current_sh="$(ps -o command -p $$)"
    case "$current_sh" in
        *bash*)
            function check_type() { type -t "$1"; }
            ;;
        *zsh*)
            function check_type() { type "$1"; }
            enable_zsh_completion ;;
        *)
            echo -e "WARNING: Only bash and zsh are supported.\nUse of other shell would lead to erroneous results."
            ;;
    esac
}
```

#### AndroidO的终端检查源码

```sh
if [ "x$SHELL" != "x/bin/bash" ]; then
    case `ps -o command -p $$` in
        *bash*)
            ;;
        *)
            echo "WARNING: Only bash is supported, use of other shell would lead to erroneous results"
            ;;
    esac
fi
```

### 选择编译目标
在编译之前我们还需要选择编译目标,所谓编译目标就是生成的镜像要运行在什么设备上,比如你是运行在Pixel手机,还是运行在虚拟机上
命令:

```sh
lunch 
```

lunch命令会将所有当前支持的编译类型都列出来,然后输入序号即可选择

![](http://47.93.60.69:88/img/pics/2018/2D58A90908894F159AFC7480B7903C09.png?x-oss-process=style/CfyInfo)

这里我们选择 28 aosp_x86_64-eng, 编译出x86版本的,大家可以根据自己的实际情况选择

![](http://47.93.60.69:88/img/pics/2018/81A66C08C91D4D5CB6C20FB2DA4A9503.png?x-oss-process=style/CfyInfo)

我们还可以使用 *choosecombo* 命令来进行一次性的选择,例如:

```sh
choosecombo 2 aosp_x86_64 3
```

![](http://47.93.60.69:88/img/pics/2018/8247C8949BCA48E0B61864D845848453.png?x-oss-process=style/CfyInfo)

choosecombo命令接收3个参数:     
- 第一个参数是 Build type:会设置TARGET_BUILD_TYPE环境变量为release或debug        
- 第二个参数是编译的product,也就是aosp_x86_64         
- 第三个参数是编译的varient,可以选择user,userdebug或者eng版本      

	- user:代表这是编译出的系统镜像是可以用来正式发布到市场的版本,其权限是被限制的,例如没有root权限,不能debug等
	- userdebug:在user版本的基础上开放了root权限和debug权限
	- eng:代表engineer,也就是所谓的开发工程师的版本,拥有最大的权限(root等),此外还附带了许多debug工具

### 编译
编译命令比较简单:

```sh
make -j18
```

通过make指令进行代码编译,该指令通过-j参数来设置参与编译的线程数量,以提高编译速度.比如这里我们设置18个线程同时编译,通常这个线程数是cpu核心数*2+2,并不是越大越好的,然后你的电脑就会编译,根据电脑性能的不同,编译的速度会有很大的区别,大约1个小时左右,应该就会编译完成

### 验证
编译完成后可以执行`emulator`命令来启动编译好的虚拟机,另外,关闭了终端,需要重新执行一遍上述的`source build/envsetup.sh` 命令和 `lunch` 命令

![](http://47.93.60.69:88/img/pics/2018/915B300164E942EFA18757312AF64286.png?x-oss-process=style/CfyInfo)

可以看到,其实这就是一个我们最常见的Android虚拟机

## 2. 导入源码到AndroidStudio
实际上Android的源码中已经专门存在了文档来告诉我们如何使用IDE来编辑Android源码,位置是 `源码路径/development/tools/idegen/README `

![](http://47.93.60.69:88/img/pics/2018/593305DF388B49228FFC5F45AE73E7AF.png?x-oss-process=style/CfyInfo)

因为我们现在都使用AndroidStudio了,所以只需要关心其中的IntelliJ部分就好了,Eclipse就不用看了

### AndroidStudio的初期配置
文档上说,由于Android太大了,所以我们需要给IDE更多的内存:
在`Help > Edit Custom VM` 中添加:

```
-Xms1g
-Xmx5g
```

![](http://47.93.60.69:88/img/pics/2018/637BEB05679446C881735A6DF4A6C0A7.png?x-oss-process=style/CfyInfo)

这两个参数的意思是初始堆内存为1G,最大堆内存为5G,其实不设置也没什么问题,但是经常会在看代码的时候,出现内存不够的错误信息,所以换个大内存还是很有必要的~

然后是AndroidStudio的类大小配置,在`Help -> Edit custom properties`中添加:

```
idea.max.intellisense.filesize=100000
```

![](http://47.93.60.69:88/img/pics/2018/5EAD5B358DC1433BAF92F33B6CCDBAEA.png?x-oss-process=style/CfyInfo)

这个参数是定义AS默认的类大小的,默认值是2500,会导致太大的Java文件不能被识别,把这个数调大了之后,就可以导入更大的Java文件了,当然还是需要一个好电脑的~     
配置完成后重启IDE

![](http://47.93.60.69:88/img/pics/2018/877849666039446BB3D4ECA606B56637.png?x-oss-process=style/CfyInfo)

### 源码导入
首先我们还是要执行一遍上述的`source build/envsetup.sh` 命令和 `lunch` 命令,当然,如果终端没有关闭的话,可以省略这一步,      
然后执行 

```sh
mmm development/tools/idegen
```

编译生成idegen.jar,这里需要注意的是,虽然google的脚本说支持zsh,但是如果你用zsh就会出现`Couldn't locate the directory development/tools/idegen` 这个错误,使用bash再来一遍就可以了

![](http://47.93.60.69:88/img/pics/2018/C70C33C118E548F3AA1A6B6C81EC01E1.png?x-oss-process=style/CfyInfo)

生成完idegen.jar之后,就可以使用命令来扫描生成ipr文件了:

```sh
sudo ./development/tools/idegen/idegen.sh
```

![](http://47.93.60.69:88/img/pics/2018/0328A6CD4E1E4F399E743D9338031056.png?x-oss-process=style/CfyInfo)

这个ipr文件就是整个项目,AndroidStudio可以直接识别打开它,就像打开正常的Android项目一样

![](http://47.93.60.69:88/img/pics/2018/3C9983968771494D8C93F320C86F3525.png?x-oss-process=style/CfyInfo)

之后AndroidStudio就开始打开项目了,这个过程会比较缓慢,有时,AS会出现如下信息:         
![](http://47.93.60.69:88/img/pics/2018/E9B307C131F14DB988E8FDC4497AF178.png?x-oss-process=style/CfyInfo)       
大致的意思就是由于项目过于庞大,现在AS没有办法很好的监视整个项目的改变了,可以通过如下方式解决:       

#### 1. 在/etc/sysctl.conf 文件末尾中添加如下代码:

> fs.inotify.max_user_watches = 524288

#### 2. 然后在终端执行以下命令:

```sh
sudo sysctl -p --system
```

最后重启AS

### AndroidStudio的其他配置
设置ProgectSDK AndroidAPI28,java版本为Java8

![](http://47.93.60.69:88/img/pics/2018/1CF5E09701624F059E8C824F68FEAEB1.png?x-oss-process=style/CfyInfo)

然后在SDK选项中仅仅保留Java1.8和Android API 28,剩下的都删除掉

![](http://47.93.60.69:88/img/pics/2018/AF70D01E3BFD41F8B795779D040F0A81.png?x-oss-process=style/CfyInfo)

![](http://47.93.60.69:88/img/pics/2018/C6EC3EC43E604B15BF602B93AB8708BE.png?x-oss-process=style/CfyInfo)
![](http://47.93.60.69:88/img/pics/2018/C6EC3EC43E604B15BF602B93AB8708BE.png?x-oss-process=style/CfyInfo)


接下来是Modules,将所有的Jar删除,因为基本上我们用不到jar,看源码就够了,如果确实需要哪个的话,再酌情保留

![](http://47.93.60.69:88/img/pics/2018/8681729585A841DAB2A4728E5BFDAAD0.png?x-oss-process=style/CfyInfo)

如果AndroidStudio一直不停 scanning files to index,可以打开 module setting --> Modules --> 找到gen文件夹  --> 右键选择Resources          
![](http://47.93.60.69:88/img/pics/2018/164CDD99E3AF4B0DB61F34AC02F68ED5.png?x-oss-process=style/CfyInfo)


现在我们就可以愉快的阅读源码啦~

### 删除不关心的项目
我们可以在AndroidStudio将我们不关心的项目或路径排除掉,这样再打开源码时就可以快一些了,例如我们将hardware路径删除     
![](http://47.93.60.69:88/img/pics/2018/F7C71AC4C84C456C951CA704C5B4C84F.png?x-oss-process=style/CfyInfo)

## 3. Debug
我们在开发的时候,势必要Debug,我们可以这样来在源码环境下调试我们的代码,以Browser2为例:

### 1. 添加断点
随便打几个断点:        
![](http://47.93.60.69:88/img/pics/2018/0D17C8CA414B46F1974928082CE991B8.png?x-oss-process=style/CfyInfo)

### 2. 点击工具栏上的Attach debugger to Android process 按钮
![](http://47.93.60.69:88/img/pics/2018/F3EE160A342445C68A8489B29A7366C0.png?x-oss-process=style/CfyInfo)   

### 3. 选择要调试的程序->OK
这里需要注意不要忘记勾选,Show all processes
![](http://47.93.60.69:88/img/pics/2018/668EB7F3EE1042D9AAE9EE1073D0090A.png?x-oss-process=style/CfyInfo)

### 4. 正常运行项目即可调试
![](http://47.93.60.69:88/img/pics/2018/46A298961B9340AF924E9291B4318B1D.png?x-oss-process=style/CfyInfo)       
可以看到我们就能正常调试我们的项目了