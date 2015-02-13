---
layout: post
title: "获取某一时刻的UIImage"
date: 2015-02-13 11:45:33 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "获取某一时刻的UIImage"
keywords: AVAssetImageGenerator, AVFoundation, AssetsLibrary
---

>代码如下：
<!--more-->
{% codeblock lang:objc%}
import UIKit
import AVFoundation
import AssetsLibrary

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        let path = NSBundle.mainBundle().pathForResource("test", ofType: "mov")
        let fileURL = NSURL(fileURLWithPath: path!)
        let avAsset = AVURLAsset(URL: fileURL, options: nil)
        
        let generator = AVAssetImageGenerator(asset: avAsset)
        generator.maximumSize = self.view.frame.size
        
        let capturedImage : CGImageRef! = generator.copyCGImageAtTime(avAsset.duration, actualTime: nil, error: nil)
        
        let imageView = UIImageView(frame: self.view.frame)
        imageView.image = UIImage(CGImage: capturedImage)
        println(avAsset.duration)
        
        self.view.addSubview(imageView)
    }
}
{% endcodeblock %}