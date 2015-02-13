---
layout: post
title: "UIView的模糊效果"
date: 2015-02-13 10:57:28 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "UIView的模糊效果"
keywords: UIBlurEffect, UIVisualEffectView
---

>效果图

{% img /images/blogImgs/uiBlurEffect_1.png 320 568 %}
{% img /images/blogImgs/uiBlurEffect_2.png 320 568 %}
{% img /images/blogImgs/uiBlurEffect_3.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit

class ViewController: UIViewController {
    
    var effectView : UIVisualEffectView!
    var mySegcon : UISegmentedControl!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.cyanColor()
        
        let image = UIImage(named: "test")
        let imageView = UIImageView(frame: self.view.bounds)
        imageView.image = image
        
        self.view.addSubview(imageView)
        
        mySegcon = UISegmentedControl(items: ["Dark", "ExtraLight", "Light"])
        mySegcon.center = CGPointMake(self.view.center.x, self.view.bounds.maxY - 50)
        mySegcon.backgroundColor = UIColor.grayColor()
        mySegcon.tintColor = UIColor.whiteColor()
        
        mySegcon.addTarget(self, action: "onClickMySegmentedControl:", forControlEvents: UIControlEvents.ValueChanged)
        self.view.addSubview(mySegcon)
    }
    
    func addVirtualEffectView(effect : UIBlurEffect!){
        if effectView != nil {
            effectView.removeFromSuperview()
        }
        effectView = UIVisualEffectView(effect: effect)
        effectView.frame = CGRectMake(0, 0, 200, 400)
        effectView.layer.position = CGPointMake(mySegcon.bounds.midX, -(effectView.frame.midY + 20) )
        effectView.layer.masksToBounds = true
        effectView.layer.cornerRadius = 20.0
        mySegcon.addSubview(effectView)
    }
    
    func onClickMySegmentedControl(sender : UISegmentedControl){
        var effect : UIBlurEffect!
        switch(sender.selectedSegmentIndex){
        case 0:
            effect = UIBlurEffect(style: UIBlurEffectStyle.Dark)
        case 1:
            effect = UIBlurEffect(style: UIBlurEffectStyle.Light)
        case 2:
            effect = UIBlurEffect(style: UIBlurEffectStyle.ExtraLight)
        default:
            println("Error")
        }
        self.addVirtualEffectView(effect)
    }
}
{% endcodeblock %}