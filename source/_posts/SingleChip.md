---
title: 嵌入式与单片机实验课答案
date: 2023-04-02 02:02:14
tags: ['学习']
top_img: /img/Top_img.jpg
cover: /img/Cover_SingleChip.png
---

# 第一次实验

## 实验基本要求

  ![](SingleChip/2023-04-02-02-05-40-image.png)

<!--more-->

## 写取平均值脚本

  创建项目文件

  ![Screenshot from 2023-04-02 00-47-22.png](SingleChip/53dd30183091b0bb68dfbdb15031cc2dfa2e0306.png)

```bash
mkdir Project //在用户目录下（~）创建Project文件夹
cd Project/  
mkdir Homework //在Project目录下创建Homework文件夹
cd Homework/
mkdir 10 //此时实验的项目地址
cd 10
```

     

  使用vim创建mean.sh文件

```bash
vim mean.sh 
```

  如果出现这种情况
  ![](SingleChip/4c9eeaf452c81e4203aab3ab35f203b530e08202.png)

```bash
sudo apt install vim
```

  安装完后在执行vim mean.sh，执行完后界面为

  ![Screenshot from 2023-04-02 01-14-51.png](SingleChip/426941f16f054370a2e48c0b9334b919349fd85a.png)

    按i键进入插入模式

  输入代码

```bash
#!/bin/bash
total=0
count=0
inputfile='1000ValuesCalcMean'
while read line;do
    total=$((total+line)）
    count=$[$count+1]
done < $inputfile
echo $[$total/$count] > mean_result_学号_名字.txt
```

      ![](SingleChip/d5a14db6ef5b8ee6e4e4628c1290b38445a7835e.png)

  输入完后按esc键退出插入模式，输入:wq退出vim

```bash
sudo chmod 777 mean.sh //给mean.sh权限
./mean.sh              //执行mean.sh
```

  执行完后10文件夹内出现 mean_result_学号_名字.txt 文件

  ![](SingleChip/57037c8bea42c86053227fe549874b419653b56c.png)

  内容为 平均数

  创建ReadMe.md

```bash
vim ReadMe.md
```

  进入后直接:wq退出

  ![Screenshot from 2023-04-02 01-14-51.png](SingleChip/426941f16f054370a2e48c0b9334b919349fd85a.png)

  看到文件夹内多出了文件ReaMe.md，双击

  ![Screenshot from 2023-04-02 01-16-19.png](SingleChip/54c588817fcd205435a974f1d8077220d5485bf4.png)

  在文本编辑器内写入以下示例后保存

  ![Screenshot from 2023-04-02 01-21-06.png](SingleChip/62d506ea99c99e2d95a5c5d0b00410291b1a7de4.png)

  打包成.tgz格式的压缩包(别忘了传图片)

```bash
tar -czvf 压缩包的名字.tgz 你要压缩的第一个文件 第二个 ....
```

  ![Screenshot from 2023-04-02 01-22-26.png](SingleChip/ba71358b9ca3c9a533af1de889672b443827b2b5.png)

  进入学习通上交即可

## 进阶要求

  代码修改为

  ![Screenshot from 2023-04-02 01-54-33.png](SingleChip/08a01404c19e5459d57c8e384f5eaf22869a43eb.png)

  执行命令改为

```bash
./mean.sh 1000ValuesCalcMean
```

  执行完后压缩上交即可(别忘了在ReadMe中说明使用了进阶要求)

# 第二次实验

## 实验要求

  ![](SingleChip/2023-04-07-15-48-18-Screenshot%20from%202023-04-07%2015-47-51.png)

  基本要求只需要配置SCP和NFS，配置SCP和NFS的文件时候添加上自己的学号截图，使用SCP和NFS的时候截图并放在Word里,把word和压缩后的配置文件上交即可

## 配置SCP

  我使用的termux，就是老师发在群里的那个app

  如果你是第一次安装termux

```bash
//termux 上执行
apt update && apt upgrade 
```

  安装Openssh

```bash
//termux 上执行
pkg install openssh
```

  安装成功后可以测试ssh的连接

```bash
//termux

//给自己的用户设置密码
passwd
//打开ssh服务端
sshd

//ubuntu

ssh -p 8022 user@hostname_or_ip
//user是你在termux上的用户可以在termux上执行whoami查看
//host_or_ip是的你ip地址，确保你的termux和ubuntu在统一局域网下
//手机按wifi查看你的ipv4地址
//也可以在termux执行ifconfig查看wlan0网络适配器的inet
```

  连接成功后,在ubuntu上随便创建一个文件测试SCP,新开一个终端

```bash
//ubuntu

scp -P 8022 SourceFile user@hostname_or_ip:TargetPath
//SourFile是你要发送的文件
//TargPath是要发送到移动的路径，termux最好用当前用户的路径，例如./ (termux没有root用户权限)
//usr是termux的用户名
//host_or_ip是的你ip地址

//在你连接termux的ssh终端或者termux上查看文件是否发送过来，如果Target是./

ls ~
```

  截图保存

## NFS

我使用的是我购买的服务器，你可以采用windos和虚拟机，或者虚拟机之间通信

```bash
sudo apt-get install nfs-kernel-server #安装NFS服务器端
sudo apt-get install nfs-common        #安装NFS客户端
```

**在服务器上**

* 设置本地目录权限

```bash
sudo mkdir /nfs
sudo chmod -R 777 /nfs
```

* 添加目录的绝对路径到共享

```bash
vim /etc/exports
#添加
/nfs *(rw,async,no_subtree_check,no_root_squash)
#学号名字
```

* 启动NFS服务

```bash
sudo /usr/sbin/exportfs -arf 
sudo /etc/init.d/nfs-kernel-server start
```

**在客户端上**

* NFS服务测试

```bash
sudo mount -t nfs 192.168.12.123:/nfs /mnt -o nolock
#如果挂载成功，在/mnt目录下能看到/nfsroot目录下的内容
#如果你就一台电脑可以自己挂载自己把 192.168.12.123 改成 127.0.0.1
```

**截图,把exports复制一份**

## 提交

这样就完成的基本要求

* 把配置文件压缩

* 把图放到word，别忘加标题

* 把这个表复制到word

![](SingleChip/2023-04-07-17-11-07-Screenshot%20from%202023-04-07%2017-10-50.png)

提交即可

## 进阶要求

可以参考我的git的使用

进阶没时间写了,找个其他时间再写

<<<<<<< Updated upstream
**这是我实验二的[文件](https://gitee.com/Exiaozhang/home-work_1/tree/master/19)包括了进阶要求的文件,可以参考着改改,不会可以线下问我,或者在我的评论区给我留言**
=======
这是我实验二的[文件]([HomeWork_1: 单片机嵌入式作业000000 - Gitee.com](https://gitee.com/Exiaozhang/home-work_1/tree/master/19))包括了进阶要求的文件,可以参考着改改,不会可以线下问我,或者在我的评论区给我留言

# 第三次实验

## 实验要求

![](SingleChip/2023-04-14-22-57-14-Screenshot%20from%202023-04-14%2022-56-57.png)

## 配置和安装VSCode

以下内容我都是在VSCode上完成的，虽然说不在VSCode上做也能完成，为了方便，我教以下怎么安装配置VSCode，方便以后的实验

进入[官网](https://code.visualstudio.com/)

Ubuntu点击下载.deb格式的安装包

![Screenshot from 2023-04-14 23-04-33.png](SingleChip/2f2a5ddf4ba91372b6ff9df6f8ee7ac110f247a2.png)

下载完成后,在下载的文件里面右键，打开终端

<img title="" src="SingleChip/2023-04-14-23-06-42-Screenshot%20from%202023-04-14%2023-06-35.png" alt="" width="505">

执行命令

```bash
#注意你的安装包的名字可能因为版本不同和我不一样
sudo apt install ./code_1.77.3-1681292746_amd64.deb 
```

安装完成后进入VSCode,进入扩展，搜索拓展

> C/C++
> 
> Makefile Tools

安装上即可

<img title="" src="SingleChip/2023-04-14-23-11-54-Screenshot%20from%202023-04-14%2023-11-20.png" alt="" width="221">

## C编程调试

在用户目录创建本实验项目的文件夹并打开VSCode

```bash
cd ~
mkdir 20
code .
```

在VSCode中新建一个hello.c文件

![Screenshot from 2023-04-14 23-15-21.png](SingleChip/d646b7e7fffae32484016a9ead05a2ffe251f2d2.png)

输入

![](SingleChip/2023-04-14-23-16-22-Screenshot%20from%202023-04-14%2023-16-08.png)

在VSCode按下`Ctrl`+`～` 打开终端中依次执行并查看运行结果

```bash
gcc –E hello.c –o hello.i
gcc –S hello.i –o hello.s
gcc –c hello.s –o hello.o
gcc hello.o –o hello
```

![](SingleChip/2023-04-14-23-37-19-Screenshot%20from%202023-04-14%2018-05-02.png)

### 使用gdb调试

生成可调试的可执行文件

```bash
gcc -g -Wall hello.c -o hello_dbg.o
```

![](SingleChip/2023-04-14-23-22-29-Screenshot%20from%202023-04-14%2018-06-22.png)

开始调试

```bash
gdb ./hello_dbg.o
```

![](SingleChip/2023-04-14-23-23-20-Screenshot%20from%202023-04-14%2018-06-28.png)

至少设置一个被调用函数内的断点并暂停（截图包括 list、break和print 被调用函数内变量，能看出是被调用函数内暂停）

```bash
#以下语句在gdb里面按顺序执行
list 
b 7
run
print a
c
q
```

![](SingleChip/2023-04-14-23-29-13-Screenshot%20from%202023-04-14%2018-06-51.png)

![](SingleChip/2023-04-14-23-29-20-Screenshot%20from%202023-04-14%2018-07-09.png)

![](SingleChip/2023-04-14-23-29-27-Screenshot%20from%202023-04-14%2021-49-34.png)

![](SingleChip/2023-04-14-23-29-32-Screenshot%20from%202023-04-14%2023-27-06.png)

![](SingleChip/2023-04-14-23-29-44-Screenshot%20from%202023-04-14%2021-49-46.png)

### gcc改为makefile

vscode新建一个Makefile文件写入

```
CC = gcc
CFLAGS = -g
OUTS = hello.out

all: ${OUTS}

%.out: %.c
    $(CC) -o $@ $^

clean:
    @rm -vf *.o *~ ${OUTS}
```

![](SingleChip/2023-04-14-23-32-58-Screenshot%20from%202023-04-14%2022-49-12.png)

使用make编译并运行

```bash
make all

./hello.out
```

![](SingleChip/2023-04-14-23-34-32-Screenshot%20from%202023-04-14%2022-49-35.png)

## 提交文件

word文档里面就是自己做了什么和截图，压缩包里面可能就是Makefile的文件把，我也不清楚，我就这样交上去的，他没说压缩包里面要有什么

# 第四次实验

## 实验要求

![](SingleChip/2023-04-15-01-35-32-Screenshot%20from%202023-04-15%2001-35-04.png)

我感觉这个实验很水，把每次实验结果截图就行

## 查看编译运行源代码

创建本次实验项目文件夹

```bash
cd ~
mkdir 30
```

把下载[文件](https://gitee.com/Exiaozhang/home-work_1/tree/master/PPT_File)你可以用的方式，把文件解压到30这个文件

![](SingleChip/2023-04-15-02-00-16-Screenshot%20from%202023-04-15%2001-59-53.png)

先在1_pthread的文件夹打开VSCode

```bash
code ~/30/1_pthread
```

**使用自己的变量重命名**（这个我觉得应该随便改改就行）

![](SingleChip/2023-04-15-02-04-59-Screenshot%20from%202023-04-15%2002-04-49.png)

随便改个名字就行，例如把上边`1.pthread_create_exit.c`中的`returned_code_err`改为`returned_code_wrong`改成这样

![](SingleChip/2023-04-15-02-08-17-Screenshot%20from%202023-04-15%2002-07-59.png)

```c
/*  线程的创建与终止 ：  */
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#define NUM_THREADS 5

void *thread_function(void *index) { /* 线程函数 */
    long tid;
    tid = (long) index;
    printf("Hello World! This is thread #%ld!\n", tid); /* 打印线程对应的参数 */
    pthread_exit(NULL);    /* 退出线程 */
}

int main(int argc, char *argv[]) {
    pthread_t tid_array[NUM_THREADS];
    int returned_code_wrong;
    long index;
    for (index = 0; index < NUM_THREADS; index++) { /* 循环创建 5 个线程 */
        printf("In main: creating thread %ld.\n", index);
        returned_code_wrong = pthread_create(
            &tid_array[index], 
            NULL, 
            thread_function, 
            (void *) index
        ); /* 创建线程 */
        if (returned_code_wrong) {
            printf("ERR: return code from pthread_create() is not 0, but %d\n", returned_code_wrong);
            exit(-1);
        }
    }
    printf("Main exits.\n");
    pthread_exit(NULL); /* 主线程退出 */
    return 0;
}
```

先清除以前编译的文件后编译代码在VSCode按下`Ctrl`+`～` 打开终端

```bash
make clean
```

![](SingleChip/2023-04-15-02-14-47-Screenshot%20from%202023-04-15%2002-14-32.png)

```bash
make all
```

![](SingleChip/2023-04-15-02-15-18-Screenshot%20from%202023-04-15%2002-15-08.png)

依次执行文件并截图

```bash
./1.pthread_create_exit.out
```

![](SingleChip/2023-04-15-02-16-23-Screenshot%20from%202023-04-15%2002-16-14.png)
>>>>>>> Stashed changes
