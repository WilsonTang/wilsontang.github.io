---
layout: post
title: "当AutoLayout遇到UIView Animation"
date: 2014-12-07 18:10:35 +0800
comments: false
sharing: false
categories: Swift

description: "当AutoLayout遇到UIView Animation"
keywords: Swift, AutoLayout, UIView Animation
---

>最近在项目中遇到一个关于`AutoLayout`与`UIView Animation`约束的问题。

`Storyboard`中, 界面布局采用了`AutoLayout`, 界面背景用2个`UIImageView`切换不同的`UIImage`, 通过`UIView Animation`来实现一个无限循环的动画效果，图层上面还有`UILabel`等控件。

<!--more-->

当我在界面上修改`UILabel`的`Font`属性的时候, 发现背景动画发生了变化。

动画受影响主要问题如下:
{% codeblock lang:objc%}
UIView.animateWithDuration(2.0, delay: 0.0, options: UIViewAnimationOptions.CurveLinear, animations: {
            self.view.layoutIfNeeded()
            }, completion: {(finish) in
        })
{% endcodeblock %}

其中用到了`self.view.layoutIfNeeded()`这个方法。

修改`UILabel`的`Font`属性的时候, 其实`self.view`也做了`layout`的处理, 这时候`UIView Animation`中也同时用到了`self.view.layoutIfNeeded()`, 造成了错乱的效果。

开始我在代码中用了

	self.view.setTranslatesAutoresizingMaskIntoConstraints(false)

结果发现进入这个界面的时候就出现来错乱, 所以肯定不能直接来手动限制`self.view`的约束。

解决办法:
将2个`UIImageView`不直接放在`self.view`上, 而是新添加一个`UIView`来放它们(比如这个`View`是`self.animationView`),  主要它们的`superView`不直接是`self.view`。这样就不会在更新`Font`属性的时候, `UIView Animation`受到影响。

代码部分修改成如下:
{% codeblock lang:objc%}
UIView.animateWithDuration(2.0, delay: 0.0, options: UIViewAnimationOptions.CurveLinear, animations: {
            self.animationView.layoutIfNeeded()
            }, completion: {(finish) in
        })
{% endcodeblock %}

