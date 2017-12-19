---
title: iOS开发-podenv工具使用指南
layout: post
date: '2017-12-07'
description: ios开发，podenv，开发工具
tag: iOS开发
---

### 前言

这篇简单介绍 podenv 的安装和使用. 如果可以看英文问答请直接看作者的文档。

[https://github.com/kylef-archive/podenv](https://github.com/kylef-archive/podenv)

### 功能
- **podenv** 用来管理多个版本的 pod在用户目录的安装和使用 ，就像rbenv用来管理多个版本的 ruby在用户目录的安装和使用。**env** 相关插件使用方式类似。
- 使用podenv在一台电脑上可以安转多个版本的pod，可以指定当前使用哪个版本。
- 而且团队合作的时候**podfile.lock** 不会因pod版本不同而引起冲突。

### 安装
```
 $ which pod       # 查找pod 安转位置      
       ### 命令行显示   /usr/local/bin/pod        
 $ sudo rm -rf  /usr/local/bin/pod    #移除当前pod 命令$       

 $ gem list    ##列出gem 安装的包，pod在列表中， pod 一般使用gem 安装
 $ sudo gem uninstall cocoapods  -v  1.3.0   ##卸载pod，注意：版本号请对应自己的 ，cocoapods-core 请勿卸载                         
 $ brew install kylef/formulae/podenv  #用home brew 安转 podenv                                      
 $ brew info podenv   #查看安装信息                                                                                              
 $ brew link --overwrite podenv   # link podenv 重新link                
 $ podenv   #使用podenv 命令 查看命令提示                                                                  
 $ podenv versions  #查看当前podenv已安装pod的版本列表  
 $ podenv version  #查看当前podenv管理使用的pod版本                                                         
 $ podenv install --list       #查看当前可以用来安装pod的版本列表                                             
 $ podenv install 1.3.1    #安装pod的某个版本  请合理选择
 $ podenv install 1.3.0    #安装pod的另外的版本
 $ podenv install 1.4.0    #安装pod的另外的版本
 $ podenv global 1.3.1   #指定当前使用1.3.1版本 团队大多数人的版本
```
### 添加环境变量
如果不添加环境变量,pod 命令可能不能使用  或者每次使用命令需要加一长串地址。
出现如下现象 ，例子：
```
  $ /usr/local/Cellar/podenv/1.0.0/libexec/pod   --version   ##  可使用
  $ /usr/local/opt/podenv/bin/pod   --versionl   ## 可使用
  $ pod   --version   ## 不可使用，找不到命令
```
编辑命令行配置文件 配置环境变量  
```
  $ vim  ~/.zshrc  # 打开zshrc 文件 编辑
```

  为了方便使用命令，请把把下面export开头的代码放到 ~/.bashrc 里, zsh用户是 ~/.zshrc 里，如果不清楚自己用的啥，那就都添加。

 按**shift + i**  对文件进入编辑状态 ，按**shift+:** 进入命令输入状态  输入wq 保存退出
 
```
 # podenv 的环境变量  把以下代码添加到~/.zshrc 文件末尾或合适位置                                                                     
 export PATH="/usr/local/opt/podenv/bin:$PATH"
```

### 测试 pod
```
$  pod            ##测试命令pod
$  pod  setup     ##pod 初始化
```
至此，就可使用podenv 来管理pod 的安装与卸载，pod多个版本可同时安装。