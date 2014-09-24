---
layout: post
title: "Create a new post"
description: "Official help page"
category: Github
tags: [Github, post, jekyll]
---
{% include JB/setup %}

近日完全转到 linux 了，不过 PocketPC 和万通宝的驱动是个大问题，Google 了好久，终于解决了万通宝的驱动，如下：

万通宝的驱动要自己编译安装，所以我们必须先要安装一些必要的软件包，以ubuntu为例，debian及其它发行版会略有不同:

我们需要的是 gcc，curl，cvs，以及与当前相对应的内核头文件

    sudo apt-get install build-essential

以上命令可安装编译所需要的软件包如 gcc，cpp等，然后是安装 cvs

    sudo apt-get install cvs

以及当前版本的内核头文件