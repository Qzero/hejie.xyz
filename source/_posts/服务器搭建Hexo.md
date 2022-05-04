---
title: 服务器搭建Hexo
top: false
abbrlink: fed583ef
date: 2021-03-17 18:17:30
tags:
categories:
---

利用GitHook进行自动化部署vps的hexo网站

<!-- more -->

## 软件支持

Git
Nginx(个人使用宝塔面板进行安装管理)
Nodejs
Hexo

## 使用过的博客系统

博客系统之前搭建网站的时候经常进行更换，一开始使用的是WordPress后来用了一段时间的Typecho最后转到了Hexo。
对比三个博客。WordPress功能最多过于庞大占用服务器资源。Typecho足够的轻量化但是编辑文章的时候比较麻烦。Hexo轻量化编辑文章方便。

## 利用githook实现一键部署

原理使用git远程部署到vps中git仓库。hexo d将生成的网页public文件夹推送到vps，vps内当git仓库收到最新的推送，将收到的文件拷贝到网站目录中。

**1，vps git搭建**

* 1安装git
个人vps使用的是ubuntu安装命令apt-git install git，安装后可通过git --version查看git版本，显示版本信息说明安装完成。

* 1.2 创建git用户
使用命令adduser git跟着设置好密码

* 1.3 赋予git sudo权限

```权限
chmod 740 /etc/sudoers
vim /etc/sudoers
```

找到root行并添加git用户如下，修改后保存退出，修改回文件权限chmod 440 /etc/sudoers

```权限
root    ALL=(ALL:ALL) ALL
git     ALL=(ALL:ALL) ALL
```

* 1.4 初始化仓库
git用户路径下创建中转裸仓库blog.git使用git --bare参数初始化裸仓库

```初始化
cd /git
mkdir blog.git
cd blog.git
git init --bare
```

* 1.5 创建网站目录
个人使用宝塔的linux面板进行创建创建管理，[官网网址](https://www.bt.cn)
使用类似宝塔linux面板的好处是方便创建网站和Nginx配置

* 1.6 配置ssh
到git用户目录下创建.ssh文件夹，将公钥复制到authorized_keys文件中。公钥地址一般在~/.ssh/id_rsa.pub。

```地址
cd /git
mkdir .ssh
cd .ssh
vim authorized_keys
```

* 1.7 确保用户组的权限为git:git
用户组内包含blog.git .ssh 网站目录三个文件夹的权限是git:git
查看权限: ll /git
更改权限：chown -R 用户名.组名 /目录

* 1.8 配置Nginx
主要是设置开启启动和网站目录路径，个人使用宝塔的linux面板进行管理

**2 配置Git Hooks**

* 2.1 创建post_-receive文件复制脚本
进入git/blog.git/hooks下创建post-receive复制脚本，赋予可执行权限chmod +x post-receive

```复制脚本
#!/bin/bash
echo "post-receive hook is running..."

GIT_REPO=/home/git/blog.git
TMP_GIT_CLONE=/tmp/blog
PUBLIC_WWW=/var/www/blog

rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

**3 本地Hexo配置**

```hexo配置
# Deployment
deploy:
    type: git
    # repo: git@VPS IP:/~/blog.git
    branch: master
```

©版权归属作者“HEJIE.XYZ / 何杰”，转载请注明。谢谢~
