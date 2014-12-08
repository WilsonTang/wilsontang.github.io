---
layout: post
title: "Mac下如何显示隐藏文件"
date: 2014-12-07 17:22:39 +0800
comments: false
sharing: false
categories: Mac

description: "Mac下如何显示隐藏文件"
keywords: Wilson Tang, Mac, 显示隐藏文件, Mac下如何显示隐藏文件
---

可以通过“终端”，用命令行设置这个选项，命令如下：
{% codeblock  lang:objc 显示%}
defaults write com.apple.finder AppleShowAllFiles -bool true
{% endcodeblock %}

{% codeblock  lang:objc 隐藏%}
defaults write com.apple.finder AppleShowAllFiles -bool false
{% endcodeblock %}