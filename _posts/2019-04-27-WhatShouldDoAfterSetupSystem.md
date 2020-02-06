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
   
   做系统盘的话，在Windows下可以用`Win32DiskImager`，Linux下直接上`dd`。

1. 用`vim.tiny`或者`vi`改源

   * **针对Manjaro系统**
   * 在`/etc/pacman.d/mirrorlist`最前面加上`Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch`
   * 修改`/etc/pacman.conf`，加上`[archlinuxcn]\nServer = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch`
   * `sudo pacman -Syy`，`sudo pacman -S archlinuxcn-keyring`，完成。
   * 完事了可以顺手`sudo pacman -Syu`一下（
   
2. 装必备软件，以下列举几个

   * `vim cmake gcc clang unzip`
   * `yay rofi`
   * `google-chrome`
   * `fcitx-im fcitx-configtool fcitx-sogoupinyin`
   * `deepin-wine-tim wps-office ttf-wps-fonts typora zotero`
   
   需要注意的是，安装完`fcitx`后，需要在`.xprofile`中添加：
   
   ```
   export XMODIFIERS="@im=fcitx"
   export GTK_IM_MODULE="fcitx"
   export QT_IM_MODULE="fcitx"
   ```
   
3. 装`zsh`，以及`oh-my-zsh`
   
   * `sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"`
   
4. 修改目录为英文

   * `sudo pacman -S xdg-user-dirs-gtk`
   * `export LANG=en_US.UTF-8`
   * `xdg-user-dirs-gtk-update`
   * 点击Update New Names，注销，Keep Old Names，完成
   * `sudo pacman -R xdg-user-dirs-gtk`（以后就用不到了，卸掉）
   
5. 交换`CapsLock`和`ESC`（vim党的刚需）

   创建`~/.Xmodmap`文件，内容：

   ```
   remove Lock = Caps_Lock
   keysym Escape = Caps_Lock
   keysym Caps_Lock = Escape
   add Lock = Caps_Lock
     ```

6. 配`.vimrc`!!!!!!

   * 从`Github`clone一份`vim-plug`：`curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim`
   * 改配置文件，安装
   * `:PlugInstall`
   * 需要补充的一点就是，我现在发现`YouCompleteMe`不管用什么插件管理器安装都很慢，所以现在我选择用`pacman`安装：
   * `sudo pacman -S vim-youcompleteme-git`，完事再整一个`.ycm_extra_conf.py`配上
   
   另外，我感觉直接装的`vim`不能读写剪贴板，有点不太够用。在`Debian`系可以装一个`vim-gtk`什么的就能解决，但是`Arch`系最佳解决方案是用`neovim`。所以在这里也提供一个`vim`到`nvim`的迁移方案（其实就是共用配置文件。
   
   ```sh
   ln -s ~/.vim ~/.config/nvim
   ln -s ~/.vimrc ~/.config/nvim/init.vim
   ```

7. 再改改系统设置就差不多了吧

   * `.i3/config`
   * `.Xresources`
   * `.config/mimeapps.list`

## Debian系

以后再补充吧，Arch真香。

## 2019-10-29 补充：Arch系统的清理

系统用了半年，终于在今天的——每天起床第一句`sudo pacman -Syu`——的时候崩了，原因是空间不足。

我的电脑是120G固态+1T机械的，装着Linux+Windows双系统，Linux作为主力，Windows作为游戏机（

为了两个系统都可以尽可能快地启动，我把128G的固态分100G给了Win10,剩下20G分给了Arch。这20G终于在今天满了，然后我就各种查资料清理空间。

再说明一下吧，我是把home分区放到机械硬盘上，其他分区放到固态上了，不这么做的话20G早就用完了(((

刚查了下，光我一个用户的home就占了20个G了……

1. 第一步，删除已经不被依赖的，之前作为依赖安装的包：

   * `sudo pacman -R $(pacman -Qqdt)`
   * 其中，`-Qqdt`的功能分别是：Query / Show less info for query / List packages installed as dependencies / List packages not required by any packages，连起来就是列出所有不被依赖的作为依赖安装的包的名字，然后`sudo pacman -R {}`删除就行了
   * 为了进一步清理，我把`-R`换成了`-Rsc`，再清理一下只被这些包依赖而不被其他包依赖的作为依赖的包
   * （好像越来越绕口了

2. 第二步，清理之前安装包的缓存及无用的数据库：

   * `sudo pacman -Sc`
   * 其中，`-Sc`的作用分别是：Sync / Remove old packages from cache，这个命令可以清理无用的软件包和无用的数据库
   * 为了进一步清理，我把`-Sc`换成了`-Scc`，这会删除缓存中所有的文件

简单两步下去，系统空间就少了一半，整整10个G，我才知道系统缓存里全都是之前更新用的package，其中大部分都（至少暂时）用不到了

哦对我还忘了说，刚才还顺便把我的kde删掉了，干掉了500M，现在基本只用i3。i3启动快，功能可以满足我的需求，而且可以完全脱离鼠标使用，我还要这个启动如此慢的kde有何用

就这样吧～

## 2020-02-06

买了台新电脑（迷你主机），打算开学后在宿舍用。

装系统的时候发现了这篇文章的一些缺陷，还有一些已经废弃的软件和我以及改变的习惯，所以把文章改了一下。

也没什么好说的了吧，就酱。

对了再补充一下，在系统中装完chrome后，chrome老是提示不是默认浏览器，点了也没用。这时候需要改`.config/mimeapps.list`，这个不太好解释，我都是装一个kde，在kde的设置里改mime，改完再把kde卸掉。我觉得有必要在`my_config`仓库里也把这个文件备份一下了。到时候直接复制粘贴就好啦～
