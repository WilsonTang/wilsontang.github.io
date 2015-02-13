---
layout: post
title: "使用CoreImage"
date: 2015-02-13 14:40:34 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "使用CoreImage"
keywords: CoreImage, CIFilter, CIImage
---

>效果图

{% img /images/blogImgs/coreImage_1.png 320 568 %}
{% img /images/blogImgs/coreImage_2.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit
import CoreImage

class ViewController: UIViewController {
    let myInputImage = CIImage(image: UIImage(named: "test.png"))
    var myImageView: UIImageView!
    let myButton: UIButton = UIButton()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        myImageView = UIImageView(frame: CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height))
        myImageView.image = UIImage(CIImage: myInputImage)
        self.view.addSubview(myImageView)
        
        myButton.frame = CGRectMake(0,0,80,80)
        myButton.backgroundColor = UIColor.grayColor();
        myButton.layer.masksToBounds = true
        myButton.setTitle("Click", forState: UIControlState.Normal)
        myButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myButton.layer.cornerRadius = 40.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height - 50)
        myButton.tag = 1
        myButton.addTarget(self, action: "onClickMyButton:", forControlEvents: .TouchUpInside)
        
        self.view.backgroundColor = UIColor.blackColor()
        self.view.addSubview(myButton);
    }
    
    func onClickMyButton(sender: UIButton){
        let myFilter = CIFilter(name: "CISepiaTone")
        
        myFilter.setValue(myInputImage, forKey: kCIInputImageKey)
        
        myFilter.setValue(1.0, forKey: kCIInputIntensityKey)
        
        var myOutputImage : CIImage = myFilter.outputImage
        
        myImageView.image = UIImage(CIImage: myOutputImage)
        
        myImageView.setNeedsDisplay()
    }
}
{% endcodeblock %}