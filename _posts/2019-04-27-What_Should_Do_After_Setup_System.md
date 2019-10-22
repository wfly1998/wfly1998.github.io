---
layout:     post
title:      "装系统之后要做的事"
subtitle:   "防止自己忘记，也为了能够在装系统之后尽快投入生产"
date:       2019-04-27 00:00:00
author:     "lee"
header-img: "img/violet-bg.jpg"
catalog:      true
multilingual: false
tags:
            - Note
---


## Arch系

0. 装系统

1. 用`vim.tiny`或者`vi`改源
   * 针对Manjaro系统
   * 在`/etc/pacman.d/mirrorlist`最前面加上`Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch`
   * 修改`/etc/pacman.conf`，加上`[archlinuxcn]\nServer = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch`
   * `sudo pacman -Syy`，`sudo pacman -S archlinuxcn-keyring`，完成。
   
2. 装必备软件，以下列举几个
   * `vim cmake make git gcc clang`
   * `yaourt`
   * `google-chrome deepin-wine-tim wps-office ttf-wps-fonts typora zotero`
   * `libsodium python-m2crypto electron-ssr`
   * `anaconda`
     * 注意：Anaconda在tuna目前已不可用，需去官网下载：
     * 20190427最新版：[Anaconda](https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh)
   * `fcitx-im fcitx-configtool fcitx-sogoupinyin`
   
3. 装`zsh`，以及`oh-my-zsh`
   
   * `sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"`
   
4. 修改目录为英文
   * `sudo pacman -S xdg-user-dirs-gtk`
   * `export LANG=en_US`
   * `xdg-user-dirs-gtk-update`
   * 点击Update New Names，注销，Keep Old Names，完成
   
5. 交换`CapsLock`和`ESC`!!!!!!!

   * 创建`~/.Xmodmap`文件，内容：

   * ```
     remove Lock = Caps_Lock
     keysym Escape = Caps_Lock
     keysym Caps_Lock = Escape
     add Lock = Caps_Lock
     ```

6. 配`.vimrc`!!!!!!

   * 从`Github`clone一份`Vundle.vim`：`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vu
     ndle.vim`
   * `:PluginInstall`
   * 编译YCM，改配置文件

7. 再改改系统设置就差不多了吧

## Debian系

以后再补充吧，Arch真香。
