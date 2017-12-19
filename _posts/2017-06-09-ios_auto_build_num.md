---
title: iOS开发-打包自动生成build号
layout: post
date: '2017-06-09'
description: ios开发，build，工具
tag: iOS开发
---

### [](http://www.vvlvv.me/#step-3)需求

#### [](http://www.vvlvv.me/#问题)问题

[原文地址:利用git 仓库的 commits 数量打包自动生成build 号](http://ikennd.ac/blog/2015/05/build-time-cfbundleversion-values-in-watchkit-apps/)http://ikennd.ac/blog/2015/05/build-time-cfbundleversion-values-in-watchkit-apps/

#### 方案

这个变通方案就是生成一个包含版本信息定义的 header 文件。然后对 Info.plist 使用预处理，来更新 app 的 Info.plist  文件build 号。

### step 1]

新建一个target，选择 “other” 下的 “Aggregate”。
[![step1](http://upload-images.jianshu.io/upload_images/2702013-7955a30c668d0008.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.vvlvv.me/images/2016-03-03-step1-new-aggregate-target.png)

### step 2

在这个新建的 target 下，创建一个 shell script 来生成 header 文件，这个文件包含 C-style #define 语句，来定义version(s)。
我的例子在这里生成了两个版本号（一个基于 git 版本库 commits 数量的版本号，一个详细版本号给出了详细的描述，commit的哈希值），然后放到build目录。
```
GIT_RELEASE_VERSION=$(git describe --tags --always --dirty --long)
COMMITS=$(git rev-list HEAD | wc -l)
COMMITS=$(($COMMITS))
mkdir -p "$BUILT_PRODUCTS_DIR/include"

echo "#define CBL_VERBOSE_VERSION ${GIT_RELEASE_VERSION#*v}" > "$BUILT_PRODUCTS_DIR/include/CBLVersions.h"

echo "#define CBL_BUNDLE_VERSION ${COMMITS}" >> "$BUILT_PRODUCTS_DIR/include/CBLVersions.h"

echo "Written to $BUILT_PRODUCTS_DIR/include/CBLVersions.h"
```
这个脚本的编译输出文件CBLVersions.h如下所示：

```
#define CBL_VERBOSE_VERSION a6f5bd0-dirty
#define CBL_BUNDLE_VERSION 1
```

![image.png](http://upload-images.jianshu.io/upload_images/2702013-6bafbc02d4f80d1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### step 3

在其他 target 的“Build Phases”面板中“Target Dependencies”添加新创建的 “aggregate target”。你可以把它添加到所有的使用 version 的 target，但你肯定会把它添加到你的 WatchKit Extension target。
[![step3](http://upload-images.jianshu.io/upload_images/2702013-bbc3aaeede37798b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.vvlvv.me/images/2016-03-03-step3-dependency-setup.png)

### step 4

Xcode 是相当聪明的，默认并行编译 target 依赖。然而，这将意味着你的 WatchKit app 将和 aggregate 在同一时间被构建，这将导致构建失败，因为 header 文件还没有及时生成。
为了解决这个问题，编辑你的 target 的 scheme，在“Build”项，取消“Parallelize Build”的选中状态。这将强制 Xcode 在继续下一步之前等待 header 文件生成。
[![step4](http://upload-images.jianshu.io/upload_images/2702013-b8028e397a3c4e46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.vvlvv.me/images/2016-03-03-step4-scheme-build-options.png)

### step 5

选中你的 target，在 build settings 中编辑如下几项：

*   **Preprocess Info.plist File** should be set to Yes.
*   **Info.plist Other Preprocessor Flags** should be set to -traditional.
*   **Info.plist Preprocessor Prefix File**  should be set to wherever your generated header file has been placed. In my case, it’s ${CONFIGURATION_BUILD_DIR}/include/CBLVersions.h.

![image.png](http://upload-images.jianshu.io/upload_images/2702013-82860910519e37e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### step 6

最后，编辑你的 Info.plist 文件，把CFBundleVersion
匹配到你生成的 header 文件。在我的例子中，我设置CFBundleVersion
 为 CBL_BUNDLE_VERSION
。

![image.png](http://upload-images.jianshu.io/upload_images/2702013-50d32acc5eedb948.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 结论

大功告成！利用git 仓库的 commits 数量打包自动生成build 号

但是这样做也有消极作用：我们需要禁用并行构建，来生成中间 header 文件和各种污秽。
在[这里](http://ikennd.ac/pictures/watchkit-versions/Clicker.zip)您可以下载本教程的项目。