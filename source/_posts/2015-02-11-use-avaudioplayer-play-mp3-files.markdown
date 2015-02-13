---
layout: post
title: "使用AVAudioPlayer播放MP3文件"
date: 2015-02-11 17:39:07 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "使用AVAudioPlayer播放MP3文件"
keywords: AVAudioPlayer, iOS use AVAudioPlayer, Swift use AVAudioPlayer, iOS AVFoundation, Swift AVFoundation
---

>效果图

{% img /images/blogImgs/avAudioPlayer_1.png 320 568 %}
{% img /images/blogImgs/avAudioPlayer_2.png 320 568 %}

>代码如下：

<!--more-->

{% codeblock lang:objc%}
import UIKit
import AVFoundation

class ViewController: UIViewController, AVAudioPlayerDelegate {
    var myAudioPlayer: AVAudioPlayer!
    var myButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let soundFilePath: NSString = NSBundle.mainBundle().pathForResource("test", ofType: "mp3")!
        let fileURL: NSURL = NSURL(fileURLWithPath: soundFilePath)!
        
        myAudioPlayer = AVAudioPlayer(contentsOfURL: fileURL, error: nil)
        myAudioPlayer.delegate = self
        
        myButton = UIButton()
        myButton.frame.size = CGSizeMake(100, 100)
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y: self.view.frame.height/2)
        myButton.setTitle("▶︎", forState: UIControlState.Normal)
        myButton.setTitleColor(UIColor.whiteColor(), forState: .Normal)
        myButton.backgroundColor = UIColor.darkGrayColor()
        myButton.addTarget(self, action: "onClickMyButton:", forControlEvents: UIControlEvents.TouchUpInside)
        myButton.layer.masksToBounds = true
        myButton.layer.cornerRadius = 50.0
        self.view.addSubview(myButton)
    }
    
    func onClickMyButton(sender: UIButton) {
        if myAudioPlayer.playing == true {
            myAudioPlayer.pause()
            sender.setTitle("▶︎", forState: .Normal)
        } else {
            myAudioPlayer.play()
            sender.setTitle("■", forState: .Normal)
        }
    }
    
    func audioPlayerDidFinishPlaying(player: AVAudioPlayer!, successfully flag: Bool) {
        println("Music Finish")
        myButton.setTitle("▶︎", forState: .Normal)
    }
    
    func audioPlayerDecodeErrorDidOccur(player: AVAudioPlayer!, error: NSError!) {
        println("Error")
    }
}
{% endcodeblock %}

