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

### 线程

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

依次执行文件并截图，后面是理解和解释的运行状态（这个要复制到word里面）

```bash
./1.pthread_create_exit.out
```

![](SingleChip/2023-04-15-02-16-23-Screenshot%20from%202023-04-15%2002-16-14.png)

pthread_create()可以创建线程，其代码如下：

```bash
void *thread_function(void *index)
e = pthread_create(
 pthread_t *thread_id,
 const pthread_attr_t *attr,
 thread_function,
 void *index
);
```

·thread_id 为所创建线程的id

·attr为线程的属性

·thread_function给所创建的线程附加内容

·index给thread_function函数提供所需要的参数

·值得注意的是，当线程创建成功时，其函数返回值为0，否则，为1。

```bash
./2.pthread_join.out 
```

![](SingleChip/2023-04-15-11-31-31-Screenshot%20from%202023-04-15%2011-30-25.png)

线程的连接

pthread_join()函数功能为等待指定线程结束，其放在主线程中目的是等待子线程结束，主线程再继续运行，其代码如下：

```
pthread_join(pthread_t thread, void **retval)
```

·thread为一指定的需要等待运行结束的线程

·retval为线程的退出码

```bash
./3.pthread_attr_init_destroy_getstack-foo_bar.out
```

![](SingleChip/2023-04-15-11-33-50-Screenshot%20from%202023-04-15%2011-33-24.png) 

属性对象的初始化与销毁

pthread_attr_init()函数用于对线程属性对象的初始化，线程具有属性，在对该结构进行处理之前必须进行初始化，其代码如下：

```c
int pthread_attr_init(pthread_attr_t *attr)
```

attr为线程属性结构体的指针变量

线程属性在使用之后就要对其进行销毁，即去初始化，要用到pthread_attr_destroy()函数，其代码如下：

```c
int pthread_attr_destroy(pthread_attr_t *attr)
```

```bash
./4.pthread_mutex.out
```

![](SingleChip/2023-04-15-11-42-08-Screenshot%20from%202023-04-15%2011-41-49.png)

互斥锁静态与动态初始化及销毁

互斥锁可用于使线程按顺序进行，其中互斥锁的初始化用到pthread_mutex_t mutex以及int pthread_mutex_init()函数，前者可静态初始化互斥锁，后者可动态初始化互斥锁其代码如下：

```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER
int pthread_mutex_init(pthread_mutex_t *restrict mutex,const pthread_mutexattr_t *restrict attr)
```

·PTHREAD_MUTEX_INITIALIZER为一结构常量

·restrict mutex为新建的互斥锁变量

·restrict attr指定了新建互斥锁的属性

·互斥锁的销毁意味着释放它所占用的资源，且要求锁当前处于开放状态。需要用到pthread_mutex_destroy()函数。由于在Linux中，互斥锁并不占用任何资源，pthread_mutex_destroy()除了检查锁状态以外没有其他动作。其代码如下：

```c
int pthread_mutex_destroy(pthread_mutex_t *mutex)
```

mutex为需要销毁的互斥锁变量

```bash
./5.pthread_dead_lock_of_mutex.out
```

![](SingleChip/2023-04-15-11-39-49-Screenshot%20from%202023-04-15%2011-39-22.png)

这里通过让两个进程不断争夺运行资源来使程序进入死锁状态。主线程创建了两个子线程pthread_1和pthread_2，p_1、p_2分别获得mutexA和mutexB，由于两个锁都上锁了，无法获得对方的锁，就造成了死锁现象。因为进入了死锁状态，所以不能继续操作了，这里使用了ctrl+z退出死锁状态。

```c
./6.pthread_conditional_variable.out
```

![](SingleChip/2023-04-15-11-44-10-Screenshot%20from%202023-04-15%2011-43-09.png)

主线程创建的两个线程会对sum_lock交替上锁，不断地打印，循环：sum++，打印，解锁互斥量，执行if语句的操作，当sum=100时，发送一个唤醒命令到一个条件变量队列等待的线程，后者会在sum大于等于100时，随时有可能接收到来自pthread_cond_signal()函数的唤醒信号使得sum=0，并打印执行信息，但无论如何主线程都会在sum与120个1相加（即前两个创建的子线程所执行的循环中i都为60时）终止线程。

### 进程

打开2_process文件夹内打开VSCode

```bash
code ~/30/2_process
```

在VSCode按`Ctrl`+`~`打开终端,先清楚编译后的代码在编译

```bash
make clean
make all
```

**和上面一样挨个运行我就不写了,直接贴图和理解了（理解和图都是要粘贴到Word里面的）**

![](SingleChip/2023-04-15-11-53-47-Screenshot%20from%202023-04-15%2011-53-37.png)

这个没有理解，也写不了

![](SingleChip/2023-04-15-11-57-23-Screenshot%20from%202023-04-15%2011-57-16.png)

这个没有理解，也写不了

![](SingleChip/2023-04-15-11-57-49-Screenshot%20from%202023-04-15%2011-57-42.png)

这里就打印出来了父进程和子进程的pid。父进程的pid是3147，子进程的pid是3148。

![](SingleChip/2023-04-15-11-59-00-Screenshot%20from%202023-04-15%2011-58-53.png)

这个没有理解，也写不了

**第五个要修改一下源代码，如果上边都是按照我的来的话，改成和我一样就行**

修改后的代码

![](SingleChip/2023-04-15-12-12-18-Screenshot%20from%202023-04-15%2012-12-12.png)

```c
/* 子进程加载新程序 */
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
char *env_init[] = {"USER=ujn", "HOME=/home/ujn/", NULL}; /* 为子进程定义环境变量 */
int main(int argc, char *argv[]) {
    pid_t pid;
    if ((pid = fork()) < 0) { /* 创建进程失败判断 */
        perror("fork error");
    } else if (pid == 0) { /* fork 对子进程返回 0 */
        execle("../../20/hello", "hello", "hello", "world", (char *) 0,env_init);/*子进程装载新程序*/
        perror("execle error"); /* execle 失败时才执行 */
        exit(-1);
    } else {
        exit(0); /* 父进程退出 */
    }
    return -1;
}
```

重新编译并运行

```bash
make all
./5-execle-example4.out 
```

![](SingleChip/2023-04-15-12-14-09-Screenshot%20from%202023-04-15%2012-13-51.png)

通过运行结果可知，子进程携带参数执行了另一个程序，输出了对应结果。

![](SingleChip/2023-04-15-12-16-14-Screenshot%20from%202023-04-15%2012-16-02.png)

检测子进程的退出状态

![](SingleChip/2023-04-15-12-18-42-Screenshot%20from%202023-04-15%2012-17-52.png)

创建守护进程并写入时间到文件中

![](SingleChip/2023-04-15-12-19-54-Screenshot%20from%202023-04-15%2012-19-49.png)

按两次ctrl+c可以退出

程序代码利用sigaction函数实现当进程首次收到SIGINT(Ctrl+c)信号时在终端打印Ouch信息，第2次按Ctrl+c时进程退出。

## 提交

**就这样就行了，后边的应该不用运行了把截图和理解粘贴到Word里面直接交上就行了**

# 第五次实验

## 实验要求

![](SingleChip/2023-04-15-13-12-39-Screenshot%20from%202023-04-15%2013-12-17.png)

## 查看编译运行源代码

老样子创建项目此实验的项目文件夹

```bash
cd ~
mkdir 40
```

把这个[上机 040 - tcp-while-neat（简洁示例）.tgz ](https://gitee.com/Exiaozhang/home-work_1/blob/master/PPT_File/%E4%B8%8A%E6%9C%BA%20040%20-%20tcp-while-neat%EF%BC%88%E7%AE%80%E6%B4%81%E7%A4%BA%E4%BE%8B%EF%BC%89.tgz)文件下载解压到这个文件夹，解压后如图

![](SingleChip/2023-04-15-13-19-19-Screenshot%20from%202023-04-15%2013-18-59.png)

这个文件夹内打开VSCode

```bash
code ~/40/tcp-while-neat
```

在VSCode新建一个Makefile文件，并输入以下内容

![](SingleChip/2023-04-15-13-21-59-Screenshot%20from%202023-04-15%2013-21-50.png)

打开终端（我上边说过怎么打开），先清除在编译代码

```bash
make clean
make all
```

![](SingleChip/2023-04-15-13-24-10-Screenshot%20from%202023-04-15%2013-23-41.png)

新开一个终端，在一个终端运行server一个运行client，点那个加号

<img src="SingleChip/2023-04-15-13-27-05-Screenshot%20from%202023-04-15%2013-26-52.png" title="" alt="" data-align="center">

```bash
#开启服务器
./server-while-tcp.out 
#开启客户端
./client.out 127.0.0.1
```

![](SingleChip/2023-04-15-13-26-17-Screenshot%20from%202023-04-15%2013-25-02.png)

![](SingleChip/2023-04-15-13-26-23-Screenshot%20from%202023-04-15%2013-25-58.png)

在客户端终端输入hello world回车

![](SingleChip/2023-04-15-13-28-48-Screenshot%20from%202023-04-15%2013-28-15.png)

返回服务器终端查看是否受到消息

![](SingleChip/2023-04-15-13-29-23-Screenshot%20from%202023-04-15%2013-28-56.png)

## 修改为多线程代码

打开服务端代码修改一下

![](SingleChip/2023-04-15-14-30-54-Screenshot%20from%202023-04-15%2014-30-27.png)

```c
#include <stdlib.h>
#include <stdio.h>
#include <errno.h>
#include <string.h>
#include <netdb.h>
#include <sys/types.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <pthread.h>

#define portnumber 3333

void *Async_ReceiveMessage(void *clientSocket)
{
    int server_session_socket = (int)clientSocket;
    int read_got_bytes_nr;
    char buffer[1024];

    if ((read_got_bytes_nr = read(server_session_socket, buffer, 1024)) == -1)
    {
        fprintf(stderr, "ERR read():%s\n", strerror(errno));
        exit(1);
    }
    buffer[read_got_bytes_nr] = '\0';
    printf("Server received %s\n", buffer); /* 这个对话服务已经结束 */
    close(server_session_socket);            /* 下一个 */
}

int main(int argc, char *argv[])
{
    int local_listen_socket, server_session_socket;
    struct sockaddr_in server_addr_info_struct;
    struct sockaddr_in client_addr_info_struct;
    int size_of_sockaddr_in;
    int read_got_bytes_nr;

    char buffer[1024];
    pthread_t ids[5];
    long t = 0;

    /* socket: 服务器端开始建立sockfd描述符 */
    if ((local_listen_socket = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    { // AF_INET i.e. IPV4; SOCK_STREAM i.e. TCP
        fprintf(stderr, "Socket error:%s\n\a", strerror(errno));
        exit(1);
    }

    /* 准备 sockaddr结构及其内部IP、端口信息 */
    bzero(&server_addr_info_struct, sizeof(struct sockaddr_in)); // 初始化,置0
    server_addr_info_struct.sin_family = AF_INET;                 // Internet
    server_addr_info_struct.sin_addr.s_addr = htonl(INADDR_ANY); // 将本机host上的long数据转化为网络上的long数据，使服务器程序能运行在不同CPU的主机上
                                                                 // INADDR_ANY 表示主机监听任意/所有IP地址。
    // server_addr_info_struct.sin_addr.s_addr=inet_addr("192.168.1.1");  //用于绑定到一个固定IP,inet_addr用于把数字加格式的ip转化为整形ip
    server_addr_info_struct.sin_port = htons(portnumber); // (将本机器上的short数据转化为网络上的short数据)端口号

    /* bind: 绑定sockfd描述符 和 IP、端口 */
    if (bind(local_listen_socket, (struct sockaddr *)(&server_addr_info_struct), sizeof(struct sockaddr)) == -1)
    {
        fprintf(stderr, "ERR bind():%s\n\a", strerror(errno));
        exit(1);
    }

    /* 设置允许连接的最大客户端数 */
    if (listen(local_listen_socket, 5) == -1)
    {
        fprintf(stderr, "ERR listen():%s\n\a", strerror(errno));
        exit(1);
    }

    while (1)
    {
        size_of_sockaddr_in = sizeof(struct sockaddr_in);
        fprintf(stderr, "Listening & Accepting...\n");
        if ((server_session_socket = accept(local_listen_socket, (struct sockaddr *)(&client_addr_info_struct), &size_of_sockaddr_in)) == -1)
        { // 服务器阻塞, 直到接受到客户连接
            fprintf(stderr, "ERR accept():%s\n\a", strerror(errno));
            exit(1);
        }

        pthread_create(&ids[t], NULL, Async_ReceiveMessage, server_session_socket);
    }

    /* 结束通讯 */
    close(local_listen_socket);
    exit(0);
}
```

修改后重新编译代码

```bash
make all
```

Split三个终端(在终端里面按住`Ctrl`+`Shift`+`5`)

![](SingleChip/2023-04-15-14-34-39-Screenshot%20from%202023-04-15%2014-34-22.png)

在一个终端运行服务器，另两个运行客户端,查看是否三连接一个客户端还能继续连接其他客户端

```bash
#服务端
./server-while-tcp.out

客户端
./client.out 127.0.0.1
```

![](SingleChip/2023-04-15-14-37-46-Screenshot%20from%202023-04-15%2014-37-36.png)

![](SingleChip/2023-04-15-14-38-52-Screenshot%20from%202023-04-15%2014-38-44.png)

## 提交文件

随便写写就行了，这是我的[文件](https://gitee.com/Exiaozhang/home-work_1/tree/d96e5afe0ca09d395114fa0526366f8e0c59b01a/40/tcp-while-neat),压缩包我是把代码压缩上传了，他也没有说压缩包里要什么
