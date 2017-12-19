---
title: Oh My Zsh 命令行工具以及powerLine主题安装
layout: post
date: '2017-06-09'
description: 开发，zsh，工具
tag: 工具
---

![](http://upload-images.jianshu.io/upload_images/2702013-39cb4f5cc52d7769.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Oh My Zsh** 是一款社区驱动的命令行工具，正如它的主页上说的：Oh My Zsh is an open source, community-driven framework for managing your [zsh](http://www.zsh.org/) configuration。它基于 zsh 命令行的一个扩展工具集，提供了丰富的扩展功能，主题配置，插件机制，已经内置的便捷操作。给我们一种全新的方式使用命令行,非常强大 。
### **Oh My Zsh主要优点：**
**1.**更强大的tab补全，当你切换目录敲两下tab，他可以列出当前目录下面的所有目录，并且可以使用键盘上下左右键来选择要进入的目录。

**2.**更智能的切换目录，比如你要进入一个很深的目录 比如：var/log/nginx/error/lastyear/may/first/monday, 用zsh可以这样输入cd /v/l/n/e/l/m/f/m，然后按tab即可补全整个路径。或者你实现知道当前目录名称，可以直接输入目录，即可进去目录。bash下cd - 可以切换到刚才进入的目录

**3.**命令选项补齐，比如输入docker，然后按tab，即可显示出docker都有哪些命令选项。

**4.**命令参数补齐，比如要kill一个进程，直接输入kill 进程名，会自动显示出进程的process id，如果用ssh，则会输出最近用ssh 连接过的主机名，配合.zshrc还可以实现自定义ping命令自动补齐的命令参数。

**更多优点**等等........具体请查看https://www.zhihu.com/question/29977255

### 一、什么是 **Oh My Zsh**
**Oh My Zsh** 它是基于 **zsh** 命令行的一个扩展工具集，提供了丰富的扩展功能。 **Oh My Zsh** 的主页上，对它的定义有了明确的解释：[http://ohmyz.sh](http://ohmyz.sh/)

关于 **zsh**，它是一种命令行程序。我们 MAC 系统上默认使用的 **bash** 命令行，而 **zsh** 是另外一种命令行环境，关于 **zsh** 大家可以到它的官网了解：

[http://www.zsh.org/](http://www.zsh.org/)

首先输入这个命令来查看我们的电脑上是否安装了 **zsh** 命令行--mac自带zsh

```
$ zsh --version
```

**注意**：如果未显示版本表示未安装zsh，先搜索查看如何安装zsh，安装成功后继续本文操作

### 二、开始安装 Oh My Zsh

主页上有很明确的说明：[http://ohmyz.sh](http://ohmyz.sh/)

可以通过 **curl** 或 **wget** 的方式即可安装。

**curl** 方式:
```
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
**wget** 方式:
```
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

安装结束会有如下提示，终端命令行的风格也已经改变
![4C1925B0-E406-4823-B11A-FAD390CCCD1A.png](http://upload-images.jianshu.io/upload_images/2702013-4def6dd499f16a9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 三、设置主题

在此我选用了网上比较流行的***“ powerLine“***  主题

#### 1、下载主题 oh-my-zsh-powerline-theme

```
$ git clone git://github.com/jeremyFreeAgent/oh-my-zsh-powerline-theme 
```

下载完后安装主题，执行目录下的脚本install.sh，此过程只是将主题powerline.zsh-theme放入~/.oh-my-zsh/themes/内，真正设置主题还需要看第四步骤：**设置oh my zsh 配置文件** ，不要急，一步一步来

```
$ sudo ./oh-my-zsh-powerline-theme/install.sh 
```

#### 2、安装主题所需要的字体，否则会乱码

![](http://upload-images.jianshu.io/upload_images/2702013-3ea6ed1b1f76f1f1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
执行以下命令
```
$ git clone https://github.com/powerline/fonts.git
```
```
$ sudo ./fonts/install.sh
```

- 到此字体安装完成，之后在终端命令行工具的偏好设置设置:
- 找到***“文本->>字体->>更改”***  ，"所有字体"中选中***“ Meslo LG M for powerLine“*** 字体


### 四、设置oh my zsh 配置文件

上一步其实还未完成主题的设置，配置主题需要进入oh my zsh配置文件***“ ~/.zshrc“***设置
Oh My Zsh 提供了很多主题风格，我们可以根据自己的喜好，设置主题风格，主题的配置在 ~/.zshrc 文件中可以看到，
```
$ vim  ~/.zshrc   //vim 编辑 zshrc 配置文件
```
修改文件此处需要用到vim命令，此处不做演示
或者用一个自己熟悉的编辑器打开这个文件，可以找到这一项：ZSH_THEME
```
ZSH_THEME="robbyrussel"  修改此项为设置主题： ZSH_THEME="powerline" 
```
```
修改此项以更好的支持自己常用命令：plugins=(git autojump osx brew node npm)   
```
保存，重启**终端命令行**即可看到powerLine 主题。
![A91DEA9F-BC50-4979-B3A0-A9263EC611F4.png](http://upload-images.jianshu.io/upload_images/2702013-f42567251f53d0a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 其他

设置常用命令别名：比如iOS开发人员经常用到pod update --verbose --no-repo-update命令
设置如下行之后，即可用pod_update 代替原来的命令
```
alias pod_update='pod update --verbose --no-repo-update'
```
#### 使用常用别名，巧用命令笔记##[请查看我的github项目](https://github.com/bosong/command_help)
**本文到此结束，谢谢！**

### 引用

*    [  你好，Oh My Zsh - 社区力量全新方式定义命令行 ](#http://www.jianshu.com/p/871ab5cb2b93)
			
*    [Oh My Zsh- github官网](#https://github.com/robbyrussell/oh-my-zsh)

*    [Powerline风格的zsh配置](#http://www.th7.cn/system/mac/201511/141085.shtml)
			

*   [功能、配置和插件 Oh My Zsh教程](#http://blog.csdn.net/a__yes/article/details/50469165)

*   [Oh My Zsh【DIY教程——亲身体验过程】](#http://www.jianshu.com/p/7de00c73a2bb)