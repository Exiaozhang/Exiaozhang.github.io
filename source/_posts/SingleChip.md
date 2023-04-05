---
title: 嵌入式与单片机实验课答案
date: 2023-04-02 02:02:14
tags:
---

# 第一次实验

* 实验基本要求
  
  ![](SingleChip/2023-04-02-02-05-40-image.png)
  
  <!--more-->
  
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
  
  * 进阶要求
  
  代码修改为
  
  ![Screenshot from 2023-04-02 01-54-33.png](SingleChip/08a01404c19e5459d57c8e384f5eaf22869a43eb.png)
  
  执行命令改为
  
  ```bash
  ./mean.sh 1000ValuesCalcMean
  ```
  
  执行完后压缩上交即可(别忘了在ReadMe中说明使用了进阶要求)

# 第二次实验
