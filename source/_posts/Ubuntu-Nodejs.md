---
title: 在Ubuntu中装Node中遇到的坑
date: 2023-04-07 15:21:44
tags:
---

# 在Ubuntu中装Nodejs

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

![](Ubuntu-Nodejs/2023-04-07-15-31-01-Screenshot%20from%202023-04-07%2015-30-53.png)

<!--more-->

结果报出GPG error:NO_PUBKEY 

* 首先我尝试了

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys YOURKEY
//把YOURKEY替换为报错提示的那个
```

* 继续执行安装代码，报出同样的错误
  
  ![](Ubuntu-Nodejs/2023-04-07-15-36-35-Screenshot%20from%202023-04-07%2015-36-21.png)

* 根据这个警告，判断apt可能没有信任deb.Nodesoure.com,执行以下代码

```bash
//找到这个仓库所在的sourelist,我是ubuntu20.4,路径可能不太一样，但都差不多
vim /etc/apt/sources.list.d/nodesource.list
//修改
deb [trusted=yes] https://deb.nodesource.com/node_18.x focal main
```

* 再次执行安装成功
  
  ![Screenshot from 2023-04-05 21-36-31.png](Ubuntu-Nodejs/1b20eb77d85e7959657003b7b219a44065b0876d.png)
