---
layout: post
title: "如何填补UIView的背景图片"
date: 2014-12-26 16:38:42 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "如何填补UIView的背景图片"
keywords: self.view.backgroundColor, How to fill background image of an UIView, UIView.backgroundColor
---

>想设置如下一张图片填充为`self.view`的背景

{% img /images/blogImgs/selfCheck_bkg.png 320 480 %}

<!--more-->

开始使用的方法是:

	self.contentView.backgroundColor = UIColor(patternImage: UIImage(named: "selfCheck_bkg")!)

效果变成了:
{% img /images/blogImgs/viewBkgFailureEffect.png 320 480 %}
明显不是我想要得到的填充效果。

解决办法,使用如下代码:
{% codeblock lang:objc%}
UIGraphicsBeginImageContext (self.view.frame.size);
UIImage(named: "selfCheck_bkg")?.drawInRect(self.view.bounds)
var bgImage:UIImage! = UIGraphicsGetImageFromCurrentImageContext()
UIGraphicsEndImageContext()
self.view.backgroundColor = UIColor(patternImage: bgImage)
{% endcodeblock %}

得到的效果变成了:
{% img /images/blogImgs/viewBkgSuccessResults.png 320 480 %}

