---
title: Hexo博客搭建与备份
date: 2018/10/21
categories:
  - 技能备忘
tags:
  - Hexo
  - 博客
abbrlink: 532cc34d
---

文 | Leon Xia

- 安装及配置[git](git-scm.com) (Windows中可用Cygwin安装)
```
sudo apt-get -y install git
```

<!--more-->

- 连接git和Github
```
git config --global user.name webstiffener
git config --global user.email web.stiffener@foxmail.com 
ssh-keygen -t rsa -C "web.stiffener@foxmail.com"
#按三次Enter，即不输入文件名或密码
#打开 ~/.ssh/id_rsa.pub
#复制打开文件中的内容，打开github并登录，点击头像、Setting、SSH and GPG keys、New SSH key、输入名称并粘贴、Add SSH key
ssh -T git@github.com
```
- 安装nodejs和npm
    - Win10: 下载安装[node.js](nodejs.org)，用`curl http://npmjs.org/install.sh | sh`安装npm
    - Ubuntu: `sudo apt install nodejs`和`sudo apt install npm`
- 安装Hexo (Ubuntu中以下命令前要加sudo)
```shell
npm config set registry="http://registry.cnpmjs.org"    #调整为国内源
npm install hexo -g    #-g指的是全局安装
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-abbrlink --save
npm install hexo-symbols-count-time --save
```
- 建立博客文件夹
    - 在github上建立名称为webstiffener.github.io的仓库，本地建立同名文件夹。
```
cd webstiffener.github.io
hexo init
npm install
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
    按照[theme-next.iissnan.com](http://theme-next.iissnan.com/)设置主题相关，主文件夹中_config.yml设置如下：
```
deploy:
  type: git
  repo: https://github.com/webstiffener/webstiffener.github.io.git
  branch: master
```
    - 如github上已存在webstiffener.github.io仓库及backup分支，则:
```shell
git clone -b backup git@github.com:webstiffener/webstiffener.github.io.git
cd webstiffener.github.io
npm install
``` 
- 预览`hexo clean && hexo generate && hexo server --draft` 
- 部署`hexo clean && hexo generate && hexo deploy`
- 备份`git push origin backup`
如出现类似`port 22: Connection time out`错误，则将.git/config文件中的url行改为：
`url = https://github.com/webstiffener/webstiffener.github.io.git`
- 安装[HexoEditor](https://github.com/zhuzhuyule/HexoEditor/releases), Win10直接下载安装, Ubuntu下[.deb安装方法](https://blog.csdn.net/qwdafedv/article/details/77836137)安装如下
```Shell
cd ~/Downloads
# 注意版本变化(下载较慢，选择FTP下载或Windows下手动用迅雷下载)
wget https://www.ofind.cn/download/HexoEditor_1.5.30_linux_x64.deb
sudo dpkg -i HexoEditor_1.5.30_linux_x64.deb.deb
#如果报依赖错误执行下面语句再试
sudo apt-get -f --fix-missing install
#如果出现Failed to load module "canberra-gtk-module"
sudo apt-get install libcanberra-gtk-module
#启动HexoEditor
sudo hexoeditor
```


