title: 'wheezy安装笔记'
tags:
  - debian
  - linux
  - wheezy
id: 1448
categories:
  - 其他
date: 2014-10-20 20:55:34
---

## 安装规划

用途： 稳定的图形/服务器开发环境
机器环境：Y470P (i5-2450M/8G/HD7690M)
Linux使用磁盘总额： 90G
磁盘分配：

|---2G  /
|---2G  /tmp
|---30G /usr
|---10G /var
|---10G /swap
|---36G /home

桌面环境：Gnome3

<!--more-->

## 准备安装

1. 下载光盘镜像: debian-7.6.0-amd64-netinst.iso
2. 将光盘镜像引导至U盘
3. AMD官网: 下载闭源驱动 fglrx
4. Intel Wifi: 下载无线网卡驱动
5. Flash Player: 下载install_flash_player_11_linux.x86_64.tar.gz
6. 网络环境

## 安装&配置软件源

1. 从U盘引导，按照提示进行最小化安装系统
2. 安装GRUB至硬盘
3. 结束安装，重启
4. 进入root
5. 更新软件源

编辑文件

```
# /etc/apt/sources.list

# deb cdrom:[Debian GNU/Linux 7 _Wheezy_ - Official Snapshot amd64 LIVE/INSTALL Binary 20140723-17:11]/ wheezy main

# deb cdrom:[Debian GNU/Linux 7 _Wheezy_ - Official Snapshot amd64 LIVE/INSTALL Binary 20140723-17:11]/ wheezy main

deb http://mirrors.163.com/debian/ wheezy main non-free contrib
deb-src http://mirrors.163.com/debian/ wheezy main non-free contrib

deb http://mirrors.163.com/debian/ wheezy-updates main non-free contrib
deb-src http://mirrors.163.com/debian/ wheezy-updates main non-free contrib
```

接着执行`apt-get update && apt-get upgrade`更新

## 安装xorg

直接安装`apt-get install xorg`

## 安装驱动

```
# copy files
cp /media/usb0/{wifi-driver}.deb ~/
cp /media/usb0/{amd-driver}.run ~/
cd ~/

# start install wifi-driver
dpkg -i ./{wifi-driver}.deb

# start install amd-driver
chmod +x {amd-driver}
./{amd-driver}.run
```

**安装AMD显卡驱动**
安装中，它可能会提示你缺少依赖组件，让你查看/usr/share/ati/fglrx-install.log。这时需要查阅缺少的组件并安装，再回去按照AMD显卡驱动。我安装时提示的是缺少gcc make linux-header三个包，利用apt安装之后OK。

## 安装桌面

为了简洁，只安装core部分，运行`apt-get install gnome-core`


## 配置中文环境

```
# reconfigure locales
# select en_GB.UTF-8 UTF-8, en_US.UTF-8 UTF-8, zh_CN GB2312, zh_CN.GB18030 GB18030, zh_CN.GBK GBK, zh_CN.UTF-8 UTF-8 
dpkg-reconfigure locales

# install chinese input
apt-get install ibus ibus-googlepinyin

# install chinese font
apt-get install xfont-wqy
```

## 安装&配置Vim

执行`apt-get install vim-gtk`

编辑文件

```
# ~/.vimrc
if has("gui_running")
  let g:isGUI = 1
else
  let g:isGUI = 0
endif

set nocompatible
syntax enable
set shortmess=atI
set autoindent
autocmd BufEnter * :syntax sync fromstart
set number
set showcmd
set lz
set hid
set incsearch
set hlsearch
set showmatch
set ai
set si
set cindent
set wildmenu
set nofen
set fdl=10

" quick shell
nmap <C-Z> :shell<cr>
inoremap <leader>n <esc>

set laststatus=2
highlight StatusLine cterm=bold ctermfg=yellow ctermbg=blue

set expandtab
set smarttab
set shiftwidth=2
set tabstop=2

set background=dark
colorscheme desert

set history=400
set autoread
set mouse=n

set encoding=utf8
set fileencodings=utf8,gb2312,gb18030,ucs-bom,latin1

" system copy cat paste
map <F7> "+y
map <F8> "+x
map <F9> "+p

" quick enter
inoremap <leader>1 ()<esc>:let leavechar=")"<cr>i
inoremap <leader>2 []<esc>:let leavechar="]"<cr>i
inoremap <leader>3 {}<esc>:let leavechar="}"<cr>i
inoremap <leader>4 {<esc>o}<esc>:let leavechar="}"<cr>O
inoremap <leader>q ''<esc>:let leavechar="'"<cr>i
inoremap <leader>w ""<esc>:let leavechar='"'<cr>i

if (g:isGUI)
  set cursorline
  colorscheme desert
  hi cursorline guibg=#333333
  hi CursorColumn guibg=#333333
  " set guifont=Consolas\ 10
  " set guifontwide=Consolas\ 10
  set guifont=Droid\ Sans\ Mono\ 12
  set gfw=Droid\ Sans\ Mono\ 12                 
  set guioptions-=T
  set guioptions-=m
else
  colorscheme desert
endif
```

## 安装浏览器Flash插件

```
# copy files
cp /media/usb0/install_flash_player_11_linux.x86_64.tar.gz ~/
cd ~/

# install plugins
tar -xvf install_flash_player_11_linux.x86_64.tar.gz
cp ./libflashplayer.so ~/.mozilla/plugins
```

## 编译＆＆安装其他包，配置各种开发环境

...