---
layout: post
title: "给UIView添加UIMotionEffect"
date: 2015-02-13 10:47:43 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "给UIView添加UIMotionEffect"
keywords: UIMotionEffect
---

>效果图

{% img /images/blogImgs/uiMotionEffect_1.png 320 568 %}
{% img /images/blogImgs/uiMotionEffect_2.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.whiteColor()
        
        let myBox = UIView(frame: CGRectMake(0,0,200,200))
        myBox.backgroundColor = UIColor.blackColor()
        myBox.layer.masksToBounds = true
        myBox.layer.cornerRadius = 20.0
        myBox.layer.position = self.view.center
        myBox.layer.zPosition = 1
        self.view.addSubview(myBox)
        
        let myLabel = UILabel(frame: CGRectMake(0,0,200,200))
        myLabel.backgroundColor = UIColor.grayColor()
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 20.0
        myLabel.text = "Hello Swift!!"
        myLabel.textColor = UIColor.whiteColor()
        myLabel.shadowColor = UIColor.grayColor()
        myLabel.textAlignment = NSTextAlignment.Center
        myLabel.layer.position = self.view.center
        myLabel.layer.zPosition = 2
        self.view.addSubview(myLabel)
        
        let xAxis1 = UIInterpolatingMotionEffect(keyPath: "center.x", type: UIInterpolatingMotionEffectType.TiltAlongHorizontalAxis)
        xAxis1.minimumRelativeValue = -100.0
        xAxis1.maximumRelativeValue = 100.0
        
        let yAxis1 = UIInterpolatingMotionEffect(keyPath: "center.y", type: UIInterpolatingMotionEffectType.TiltAlongVerticalAxis)
        yAxis1.minimumRelativeValue = -100.0
        yAxis1.maximumRelativeValue = 100.0
        
        let group1 = UIMotionEffectGroup()
        group1.motionEffects = [xAxis1, yAxis1]
        
        let xAxis = UIInterpolatingMotionEffect(keyPath: "center.x", type: UIInterpolatingMotionEffectType.TiltAlongHorizontalAxis)
        xAxis.minimumRelativeValue = -50.0
        xAxis.maximumRelativeValue = 50.0
        
        let yAxis = UIInterpolatingMotionEffect(keyPath: "center.y", type: UIInterpolatingMotionEffectType.TiltAlongVerticalAxis)
        yAxis.minimumRelativeValue = -50.0
        yAxis.maximumRelativeValue = 50.0
        
        let group = UIMotionEffectGroup()
        group.motionEffects = [xAxis, yAxis]
        
        myBox.addMotionEffect(group)
        myLabel.addMotionEffect(group1)
    }
}
{% endcodeblock %}
