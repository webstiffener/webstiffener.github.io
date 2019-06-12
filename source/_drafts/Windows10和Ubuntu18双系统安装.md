---
title: Windows10和Ubuntu18.04双系统安装
date: 2019/06/11
categories:
  - 技能备忘
tags:
  - ubuntu
  - Windows10
  - 系统安装
abbrlink: 17565
---

文 | Leon Shia

> 由于需要PE系统盘和安装系统盘，所以最好有两个U盘，其中建议PE系统盘可使用Drivedroid → + → Create Blank Img作为U盘使用。正式安装前，可重启电脑按F12进入启动设置，选择对应盘看是否能启动。 
> 如果只用一个盘，可百度使用PE系统装机工具来安装。

<!--more-->

# 金狐PE系统
- 从百度网盘或金狐网站下载PE系统压缩包，解压密码一般为uqi
- 丛中解压出系统.iso文件和刻录工具UltraISO.exe
- UltraISO.exe → 
    - 点击安装系统.iso文件
    - 启动 → 写入硬盘映像
    - 硬盘驱动器 → 选择U盘 (可使用Drivedroid → + → Create Blank Img作为U盘使用）
    - 便捷启动 → 便捷写入
    - 写入

# Windows10
## 制作启动盘
- 插入U盘并格式化，文件系统选FAT32 (可使用Drivedroid → + → Create Blank Img作为U盘使用)
- 可跳过后面的步骤，从百度网盘中下载.pmf文件，直接执行DiskGenius → 从镜像文件还原分区
- 启动MediaCreationTool按指示制作启动盘
- [将U盘分为启动盘和存储盘](https://www.jb51.net/softjc/260117.html)(按需，可跳过)
    - 启动DiskGenius，选择U盘右键，备份分区到镜像文件
    - 右键，删除当前分区
    - 右键，建立新分区（扩展磁盘分区，所有空间）
    - 右键，建立新分区（逻辑分区，NTFS，大小为用于存贮空间）
    - 右键，建立新分区（逻辑分区，**FAT**，大小为剩余空间，略大于备份文件）
    - 左上角，保存更改，按提示格式化分区
    - 右键第5步中的分区，转换为主分区
    - 右键第5步中的分区，格式化分区（勾选建立DOS系统）
    - 右键第5步中的分区，从镜像文件还原分区，找到第1步中的备份文件

## 预留空间和安装
- 重启电脑，按F1进入Bios → 
    - Config → Serial ATA → SATA Controller Mode Option → AHCI
    - Startup → UEFI/Legacy Boot → UEFI Only
- 插入PE盘，重启，按F12进入启动设置，选择对应PE盘，进入维护系统
- 启动PE系统中的DiskGenius → 选中SSD硬盘(系统安装目标盘) → 删除分区 → 快速分区(如果装双系统则分区数目=2) 

{% asset_img DiskGenius.jpg %}

- 插入Win10启动盘，重启，按F12进入启动设置，选择对应Win10启动盘，等待安装完成
- 使用DriveGenius安装驱动

## 激活
右键开始 → 命令提示符（管理员）→
```Shell
slmgr /ipk VK7JG-NPHTM-C97JM-9MPGT-3V66T
slmgr /skms kms.xspace.in
slmgr /ato
```

## OFFICE 2010
- 百度盘下载Office2010专业版和KMS激活软件
- 使用「试用密钥」安装，并用KMS激活 (有效期为6个月，后需重新激活)。杀毒软件会报错，设置成安全即可。

## 安装Cygwin
- 下载安装Cygwin，安装时选中wget、tmux、dos2unix、git、vim、openssh、curl
- 执行
- 将地址[git-completion](https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash)的代码保存为`C:\cygwin64\home\XXX\.git-completion.bash`
- 在`C:\cygwin64\home\XXX\.bashrc`加入
```
source ~/.git-completion.bash
cd f:\workspace
#aloas 是将相应的命令缩写，方便使用
alias ws='cd f:\workspace'
alias hexos='hexo clean && hexo generate && hexo server --draft'
alias hexod='hexo clean && hexo generate && hexo deploy'
```
- 去掉`C:\cygwin64\home\XXX\.inputrc`文件中`set completion-ignore-case on`行前面的注释符号#
- 其它
```git
git config --system core.fileMode false
git config --system core.quotepath false
git config --system color.ui true
git config --global user.name "webstiffener"
git config --global user.email web.stiffener@foxmail.com
git config --system alias.st status
git config --system alias.ci "commit -s"
git config --system alias.co checkout
git config --system alias.br branch
git config --system alias.ll "log --pretty=fuller --stat --graph --decorate"
git config --system alias.ls "log --pretty=oneline --graph --decorate"
git config --system alias.ss "status -sb"
```

## 安装Hexo
参考《Hexo博客搭建与备份》

# Ubuntu18.04
- 百度盘或官网下载最新系统，将下载的Ubuntu系统「解压」到U盘即可
- 右键我的电脑 → 属性 → 控制面板主页 → 硬件和声音 → 电源选项/更改电源按钮的功能 → 更改当前不可用的设置 →取消勾选'快速启动'(如不存在该项则忽略)
- 重启电脑，按F1进入Bios，找到Security Boot并设置为Disabled，保存退出
- 插入安装盘，重启，按F12进入启动设置，选择对应U盘(此处如使用Drivedroid可能无法识别)
- 选择安装Ubuntu系统，选择语言等选项，**不要联网**
- 安装类型选择「其它选项(创建自己的分区)」 → 选择预留的空间 → 点击+ 
    - 大小为电脑内存大小(8GB)，主分区，空间起始位置，swap
    - 大小为512MB，逻辑分区，空间起始位置，efi系统分区
    - 大小为50GB，逻辑分区，空间起始位置，EXT4日志文件系统，/home
    - 大小为30GB，逻辑分区，空间起始位置，EXT4日志文件系统，/usr
    - 剩余10GB+，逻辑分区，空间起始位置，EXT4日志文件系统，/ 

**重点：安装引导启动器的设备 → 以上「efi系统分区」对于盘符**
- 等待安装完成
- 参考《Ubuntu安装完成后的一些配置》进行配置
- 重启电脑，按F1进入Bios，找到Security Boot并改回Enabled，保存退出

