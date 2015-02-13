---
layout: post
title: "使用AVFoundation启动相机并保存"
date: 2015-02-13 11:30:57 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "使用AVFoundation启动相机并保存"
keywords: AVFoundation, AVCaptureSession, AVCaptureDevice, AVCaptureStillImageOutput, AVCaptureDeviceInput, AVCaptureVideoPreviewLayer
---

>效果图

{% img /images/blogImgs/avFoundation_1.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit
import AVFoundation

class ViewController: UIViewController {
    var mySession : AVCaptureSession!
    var myDevice : AVCaptureDevice!
    var myImageOutput : AVCaptureStillImageOutput!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        mySession = AVCaptureSession()
        
        let devices = AVCaptureDevice.devices()
        
        for device in devices{
            if(device.position == AVCaptureDevicePosition.Back){
                myDevice = device as AVCaptureDevice
            }
        }
        
        let videoInput = AVCaptureDeviceInput.deviceInputWithDevice(myDevice, error: nil) as AVCaptureDeviceInput
        
        mySession.addInput(videoInput)
        
        myImageOutput = AVCaptureStillImageOutput()
        
        mySession.addOutput(myImageOutput)
        
        let myVideoLayer : AVCaptureVideoPreviewLayer = AVCaptureVideoPreviewLayer.layerWithSession(mySession) as AVCaptureVideoPreviewLayer
        myVideoLayer.frame = self.view.bounds
        myVideoLayer.videoGravity = AVLayerVideoGravityResizeAspectFill
        
        self.view.layer.addSublayer(myVideoLayer)
        
        mySession.startRunning()
        
        let myButton = UIButton(frame: CGRectMake(0,0,120,50))
        myButton.backgroundColor = UIColor.redColor();
        myButton.layer.masksToBounds = true
        myButton.setTitle("拍摄", forState: .Normal)
        myButton.layer.cornerRadius = 20.0
        myButton.layer.position = CGPoint(x: self.view.bounds.width/2, y:self.view.bounds.height-50)
        myButton.addTarget(self, action: "onClickMyButton:", forControlEvents: .TouchUpInside)
        
        self.view.addSubview(myButton);
    }
    
    func onClickMyButton(sender: UIButton){
        let myVideoConnection = myImageOutput.connectionWithMediaType(AVMediaTypeVideo)
        self.myImageOutput.captureStillImageAsynchronouslyFromConnection(myVideoConnection, completionHandler: { (imageDataBuffer, error) -> Void in
            let myImageData : NSData = AVCaptureStillImageOutput.jpegStillImageNSDataRepresentation(imageDataBuffer)
            let myImage : UIImage = UIImage(data: myImageData)!
            UIImageWriteToSavedPhotosAlbum(myImage, self, nil, nil)
        })
    }
}
{% endcodeblock %}