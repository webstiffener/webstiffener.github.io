---
abbrlink: '0'
---
1. 添加源
```Shell
sudo gedit /etc/apt/sources.list
```
将以下内容添加到打开的文件中
```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security multiverse
deb http://archive.ubuntukylin.com:10006/ubuntukylin trusty main
```
默认中国服务器，我们把它切换成aliyun的。
在设置--软件和更新里--下载自--其他站点--中国--http://mirrors.aliyun.com/ubuntu

2. 其他（可将以下内容存为X.sh文件，使用`bash X.sh`执行）
```Shell
#更新
sudo apt-get update
sudo apt-get --assume-yes upgrade

#删除不常用软件
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install onboard deja-dup

#安装及配置git
sudo apt-get -y install git
git config user.name webstiffener
git config user.email web.stiffener@foxmail.com
ssh-keygen -t rsa -C "web.stiffener@foxmail.com"
#按三次Enter，即不输入文件名或密码
gedit ~/.ssh/id_rsa.pub
#复制打开文件中的内容，打开github并登录，点击头像、Setting、SSH and GPG keys、New SSH key、输入名称并粘贴、Add SSH key
ssh -T git@github.com

#删除原有输入法，为安装搜狗输入法做准备
sudo apt-get remove fcitx*
sudo apt-get autoremove
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get -y install fcitx
sudo apt-get install fcitx-config-gtk
sudo apt-get -y install fcitx-table-all
sudo apt-get install im-switch
#下载搜狗输入法并安装(下载地址可能会变化)，安装好后在右上角配置优先顺序
wget http://cdn2.ime.sogou.com/dl/index/1509619794/sogoupinyin_2.2.0.0102_amd64.deb
sudo dpkg -i sogoupinyin_2.2.0.0102_amd64.deb
sudo apt-get -y install -f
sudo rm sogoupinyin_2.2.0.0102_amd64.deb

#安装视频播放器VLC
sudo apt-get -y install vlc

#安装Flash Player
mkdir -p flash_install
cd flash_install
wget https://fpdownload.adobe.com/get/flashplayer/pdc/28.0.0.126/flash_player_npapi_linux.x86_64.tar.gz
tar -zxvf flash_player_npapi_linux.x86_64.tar.gz
sudo cp libflashplayer.so /usr/lib/firefox/browser/plugins
sudo cp -r usr/* /usr
cd ..
sudo rm -r flash_install

#安装美化工具
sudo apt-get -y install unity-tweak-tool

#安装思源黑体
mkdir -p noto_font_install
cd noto_font_install
wget https://noto-website.storage.googleapis.com/pkgs/Noto-hinted.zip
unzip Noto-hinted.zip
mkdir -p ~/.fonts/noto
mv *.otf ~/.fonts/noto
wget  http://files.cnblogs.com/daijkstra/fonts.conf.zip
unzip fonts.conf.zip -d ~/
cd ..
sudo rm -r noto_font_install
```

3. 安装深色主题及图标，参考[主题设置](https://jingyan.baidu.com/article/63f2362859821b0208ab3d04.html)
```Shell
sudo add-apt-repository ppa:noobslab/themes
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install arc-flatabulous-theme
sudo apt-get install ultra-flat-icons
```
4. github博客相关
```Shell
sudo apt-get install node.js
sudo apt-get install npm
sudo apt-get install hexo
git clone https://github.com/webstiffener/webstiffener.github.io
```

5. Fast.ai环境配置
```Shell
#第一句是确保安装了git，最后一句是设置jupyter notebook密码
sudo apt-get -y install git
wget https://github.com/fastai/courses/blob/master/setup/install-gpu.sh
bash install-gpu.sh
jupyter notebook password
```

6. Tmux和[tmux插件](https://liam0205.me/2016/09/10/tmux-plugin-resurrect/)的安装与配置
```
sudo apt-get install tmux
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
配置文件~/.tmux.conf
（Windows下用Cygwin安装Tmux后，在Cygwin的Home文件夹中创建.tmux.conf文件并使用`tmux source-file ~/.tmux.conf`加载配置文件
```
# Send prefix 
set-option -g prefix C-a 
unbind-key C-a 
bind-key C-a send-prefix 

# Use Alt-arrow keys to switch panes 
bind -n M-Left select-pane -L 
bind -n M-Right select-pane -R 
bind -n M-Up select-pane -U 
bind -n M-Down select-pane -D 

# Shift arrow to switch windows 
bind -n S-Left previous-window 
bind -n S-Right next-window 

# Mouse mode 
set -g mouse on 

# Set easier window split keys 
bind-key v split-window -h 
bind-key h split-window -v 

# Easy config reload 
bind-key r source-file ~/.tmux.conf \; display-message "tmux.conf reloaded"



# Ctr+B + 'shift-i', 'shift-u', 'alt-u'分别为安装，更新，反安装以下列表中的插件
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'

# 配置tmux-resurrect
# Ctr+B + 'Ctr+S', 'Ctr+S'分别为保存和加载Tmux现场信息
set -g @resurrect-save-shell-history 'on'
set -g @resurrect-capture-pane-contents 'on'
set -g @resurrect-strategy-vim 'session'


# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```
