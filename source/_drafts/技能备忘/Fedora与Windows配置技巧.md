---
abbrlink: '0'
---
1. Fedora Flash-Plugin安装
前两行是删除已经安装的Flash插件相关的东西。
```Shell
# dnf remove flash-plugin nspluginwrapper* -y
# dnf remove adobe-release-x86_64-1.0-1.noarch
# rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-x86_64-1.0-1.noarch.rpm
# dnf install flash-plugin
```
2. R和Rstudio的安装
Rstudio的下载网址可在 [官网下载页面](https://www.rstudio.com/products/rstudio/download/) 查看。
```Shell
# dnf install R-base
# rpm -ivh https://download1.rstudio.org/rstudio-1.0.143-x86_64.rpm
```
3. Win10激活方法
右键开始→命令提示符（管理员）→
```Shell
slmgr /ipk VK7JG-NPHTM-C97JM-9MPGT-3V66T
slmgr /skms kms.xspace.in
slmgr /ato
```
4. Win10下Libreoffice
安装JDK环境开发环境→设置Java环境变量→安装Libreoffice→安装Language Tool
（PAN://NEW/Libreoffice Installation)

5. 解决grub rescue等问题
插入Ubuntu安装盘(可用drivedroid下载并制作)→开机按Fn+F1进入bios→在startup下的boot中选择USB HDD→开机进入安装程序→选择Try Ubuntu(不安装)→进入系统打开命令行→
```Shell
sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt-get update
sudo apt-get install -y boot-repair
sudo boot-repair
```
点击recommanded repair
执行第一行需要一点时间，长时间没反应可按几下Enter按键

6. Windows10下安装Cygwin
- 下载安装Cygwin，安装时选中wget、tmux、dos2unix、git、vim、openssh
- 执行
```Shell
dos2unix /cygdrive/c/Fastai/anaconda3-4.4.0/Scripts/activate.bat`
dos2unix /cygdrive/c/Fastai/anaconda3-4.4.0/Scripts/deactivate.bat
```
- 将地址[git-completion](https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash)的代码保存为`C:\Fastai\cygwin64\home\Xialiang Chen\.git-completion.bash`，并在该目录下的`.bashrc`文件中加入`source ~/.git-completion.bash`
- 在`C:\Fastai\cygwin64\home\Xialiang Chen\.bashrc`加入`cd f:\workspace`
- 去掉`C:\Fastai\cygwin64\home\Xialiang Chen\.inputrc`文件中`set completion-ignore-case on`行前面的注释符号#
- 其它：
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

#连接github
ssh-keygen -t rsa -C "web.stiffener@foxmail.com"
```
