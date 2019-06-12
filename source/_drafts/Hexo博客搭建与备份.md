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
#以下两行只能在一个仓库文件夹下执行成功
git config user.name webstiffener
git config user.email web.stiffener@foxmail.com 
ssh-keygen -t rsa -C "web.stiffener@foxmail.com"
#按三次Enter，即不输入文件名或密码
#打开 ~/.ssh/id_rsa.pub
#复制打开文件中的内容，打开github并登录，点击头像、Setting、SSH and GPG keys、New SSH key、输入名称并粘贴、Add SSH key
ssh -T git@github.com
```
- 下载安装[node.js](nodejs.org)，用`curl http://npmjs.org/install.sh | sh`安装npm
- 安装Hexo
```shell
npm config set registry="http://registry.cnpmjs.org"    #调整为国内源
npm install hexo -g    #-g指的是全局安装
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