---
layout: post
title: "使用系统Social"
date: 2015-02-13 14:54:01 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "使用系统Social"
keywords: Social, SLComposeViewController
---

>效果图

{% img /images/blogImgs/share_1.png 320 568 %}
{% img /images/blogImgs/share_2.png 320 568 %}
{% img /images/blogImgs/share_3.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit
import Social

class ViewController: UIViewController {
    var myComposeView : SLComposeViewController!
    var myShareButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let hex: Int = 0x55ACEE
        let red = Double((hex & 0xFF0000) >> 16) / 255.0
        let green = Double((hex & 0xFF00) >> 8) / 255.0
        let blue = Double((hex & 0xFF)) / 255.0
        var myColor: UIColor = UIColor( red: CGFloat(red), green: CGFloat(green), blue: CGFloat(blue), alpha: CGFloat(1.0))
        
        myShareButton = UIButton()
        myShareButton.frame = CGRectMake(0,0,100,100)
        myShareButton.backgroundColor = myColor
        myShareButton.layer.masksToBounds = true
        myShareButton.setTitle("Share", forState: UIControlState.Normal)
        myShareButton.titleLabel?.font = UIFont.systemFontOfSize(CGFloat(20))
        myShareButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myShareButton.layer.cornerRadius = 50.0
        myShareButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height/2)
        myShareButton.tag = 1
        myShareButton.addTarget(self, action: "onPostToShare:", forControlEvents: .TouchUpInside)
        
        self.view.addSubview(myShareButton)
    }
    
    func onPostToShare(sender : AnyObject) {
        //分享Twitter, 新浪微博， 腾讯微博
        myComposeView = SLComposeViewController(forServiceType: SLServiceTypeTwitter)
//        myComposeView = SLComposeViewController(forServiceType: SLServiceTypeTencentWeibo)
//        myComposeView = SLComposeViewController(forServiceType: SLServiceTypeSinaWeibo)
        
        myComposeView.setInitialText("Share Test from Swift")
        
        myComposeView.addImage(UIImage(named: "test.png"))
        
        self.presentViewController(myComposeView, animated: true, completion: nil)
    }
}
{% endcodeblock %}