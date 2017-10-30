title: Linux Shell Summary
date: 2017-09-03 21:33:01
tags: [linux, notes, shell]
categories: summary
description: Linux shell summary。
---

# Linus Shell Summary

## 查看端口使用情况：
`sudo lsof -i:8888`

若要停止使用这个端口的程序，使用kill +对应的pid即可

## Copy Public Key :

`ssh-agent` //启动 ssh-agent，大家可以自行搜索自己所使用OS启动 ssh-agent的方式。

`ssh-add`

`ssh-add -l` // 检查自己的私钥是否被ssh-agent管理

执行以下命令，将本地public-key添加到的authorized_keys里面：

`ssh-copy-id -i ~/.ssh/id_rsa.pub User@HostName`

# 查找gz文件中的log
zgrep zcat

- `zgrep 60a3b7146b12 laindocker.log-20170506.gz > ~/T1836-all`

## 从服务器上 (下载|上传) 文件到本地
- 下载： `scp xxx@gpu:/home/src /home/drc`

若是目录的话， 加 `-r`

- 上传： `scp -r /localpath xxx@gpu:remotePath`


## ubuntu soft link

文件夹建立软链接（用绝对地址）

ln -s 源地址 目的地址

比如我把linux文件系统rootfs_dir软链接到/home/xxx/目录下

　　`ln -s /opt/linux/rootfs_dir  /home/xxx/rootfs_dir`
　　
## 查看系统版本

`cat /etc/issue `  

--- 
　　