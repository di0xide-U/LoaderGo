# LoaderGo

LoaderGo-快速生成免杀木马GUI版本，bypass主流杀软

## 开发背景

最近在学习免杀，学了如何使用go来实现shellcode加载器，于是打算写一款gui版本的shellcode加载器。实测过 bypass火绒、金山毒霸、360全家桶、360核晶、wdf、迈克菲等主流杀软

可视化界面参考了wails：https://wails.io/zh-Hans/。 

本LoaderGo只公开了一些基础加载方式，明年开始会陆续更新更多加载方式。但总体来说，使用go来写会遇到各种依赖问题，后续尽量转移到C语言上。

![image-20231217004838072](./Readme.assets/image-20231217004838072.png)

## 注意事项

1. **在此声明，未经授权不要拿去做违法事情，若出任何事情与本人无关。**
2. 不用传到微步之类的沙箱，LoaderGo运行环境最好断网，不要被上传样本导致木马很快被杀。

## 环境准备

虽然LoaderGo程序本身已经通过Go编译好了，但是在生成exe的时候，还是需要go环境，因此运行环境需要有go1.20版本。

环境准备：Go和git，Go：1.20.x版本

https://go.dev/dl/go1.20.12.windows-amd64.msi 下载安装go

![image-20231217002139389](./Readme.assets/image-20231217002139389.png)

如果环境内有多个版本Go，可以使用g:https://github.com/voidint/g来进行管理，亲测好用

![image-20231217002327817](./Readme.assets/image-20231217002327817.png)

https://git-scm.com/download/win 安装git

https://github.com/git-for-windows/git/releases/download/v2.43.0.windows.1/Git-2.43.0-64-bit.exe

![image-20231217004628761](./Readme.assets/image-20231217004628761.png)



打开LoaderGo的时候可能会提示没有webview组件，正常安装即可。

![image-20231217010308456](./Readme.assets/image-20231217010308456.png)

![image-20231217010315190](./Readme.assets/image-20231217010315190.png)

## 使用方法

### 初始化

打开我们的工具，这里提示我们没有依赖

![image-20231216201253493](./Readme.assets/image-20231216201253493.png)

点击菜单安装依赖，或者点击目录下的安装依赖.bat都可以。

![image-20231216201620031](./Readme.assets/image-20231216201620031.png)

这里要注意，依赖是基于Go1.20版本的，所以本机环境必须是1.20的go。

依赖安装完毕

![image-20231216201714906](./Readme.assets/image-20231216201714906.png)

如果这里点击以后，界面卡住的话，请手动点击依赖.bat

根据提示再次初始化

![image-20231216201745431](./Readme.assets/image-20231216201745431.png)

初始化成功后，就可以开始生成shellcode了，这里可以点击清除日志，让界面干净一些。

![image-20231216201833667](./Readme.assets/image-20231216201833667.png)

### 生成shellcode

使用cs生成payload

![image-20231216201007689](./Readme.assets/image-20231216201007689.png)

这里选择C

![image-20231216201041149](./Readme.assets/image-20231216201041149.png)

删除多余的信息，只留\x00\x11\x99…….这些字符

![image-20231216201338699](./Readme.assets/image-20231216201338699.png)

### 加密

LoaderGo采取的是远程分离加载，我们先对原生shellcode进行加密。

导入shellcode，选择加密方式，这里只写了一种，所以不用选择，直接生成即可。

![image-20231216202603921](./Readme.assets/image-20231216202603921.png)

将生成的文件放在云上或者vps上，为了隐匿，推荐使用oss。

![image-20231216202708875](./Readme.assets/image-20231216202708875.png)

这里我放到了云端上，接下来就是生成exe了

### 生成木马

把刚才云端的地址，填到这里

![image-20231216203151469](./Readme.assets/image-20231216203151469.png)

选择想要的加载方式和解密模式，这里因为只有一种加密所以这里不用选别的。然后填上前一步生成的密钥

![image-20231216203332730](./Readme.assets/image-20231216203332730.png)

然后选择反沙箱模式，一般情况下选择1和3就可以，选2的话将在虚拟机里无法执行。然后点击生成即可。等待约30s即可生成到指定目录。

![image-20231216203532212](./Readme.assets/image-20231216203532212.png)



## 免杀效果

### 360全家桶

四种加载方式都可以上线

![image-20231216213716952](./Readme.assets/image-20231216213716952.png)

开启所有引擎

![image-20231216214020539](./Readme.assets/image-20231216214020539.png)

![image-20231216210309491](./Readme.assets/image-20231216210309491.png)

线程注入也可以成功上线

![image-20231216211000090](./Readme.assets/image-20231216211000090.png)

### 金山毒霸+火绒

![image-20231216232607785](./Readme.assets/image-20231216232607785.png)

![image-20231216212206787](./Readme.assets/image-20231216212206787.png)

![image-20231216212310939](./Readme.assets/image-20231216212310939.png)



### wdf

![image-20231216214633877](./Readme.assets/image-20231216214633877.png)

![image-20231216214700031](./Readme.assets/image-20231216214700031.png)

### 卡巴斯基

前两种会被直接删除，线程注入的可以正常上线，但是躲不过内存扫描（后续继续完善）

![image-20231216220630933](./Readme.assets/image-20231216220630933.png)

![image-20231216220648647](./Readme.assets/image-20231216220648647.png)

![image-20231216221055367](./Readme.assets/image-20231216221055367.png)



### 迈克菲

![image-20231217000043090](./Readme.assets/image-20231217000043090.png)

![image-20231217000158517](./Readme.assets/image-20231217000158517.png)

