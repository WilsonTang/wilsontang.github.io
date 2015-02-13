---
layout: post
title: "拍摄视频"
date: 2015-02-13 11:33:35 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "拍摄视频"
keywords: AVFoundation, AVCaptureFileOutputRecordingDelegate, AVCaptureSession, AVCaptureDevice, AVCaptureStillImageOutput, AVCaptureMovieFileOutput, AVCaptureVideoPreviewLayer
---

>效果图

{% img /images/blogImgs/avFoundation_2.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit
import AVFoundation

class ViewController: UIViewController, AVCaptureFileOutputRecordingDelegate {
    var mySession : AVCaptureSession!
    var myDevice : AVCaptureDevice!
    var myImageOutput : AVCaptureStillImageOutput!
    var myVideoOutput : AVCaptureMovieFileOutput!
    var myButtonStart : UIButton!
    var myButtonStop : UIButton!
    
    
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
        
        let audioCaptureDevice = AVCaptureDevice.devicesWithMediaType(AVMediaTypeAudio)
        
        let audioInput = AVCaptureDeviceInput.deviceInputWithDevice(audioCaptureDevice[0] as AVCaptureDevice, error: nil)  as AVCaptureInput
        
        mySession.addInput(audioInput);
        
        myImageOutput = AVCaptureStillImageOutput()
        
        mySession.addOutput(myImageOutput)
        
        myVideoOutput = AVCaptureMovieFileOutput()
        
        mySession.addOutput(myVideoOutput)
        
        let myVideoLayer : AVCaptureVideoPreviewLayer = AVCaptureVideoPreviewLayer.layerWithSession(mySession) as AVCaptureVideoPreviewLayer
        myVideoLayer.frame = self.view.bounds
        myVideoLayer.videoGravity = AVLayerVideoGravityResizeAspectFill
        
        self.view.layer.addSublayer(myVideoLayer)
        
        mySession.startRunning()
        
        myButtonStart = UIButton(frame: CGRectMake(0,0,120,50))
        myButtonStop = UIButton(frame: CGRectMake(0,0,120,50))
        
        myButtonStart.backgroundColor = UIColor.redColor();
        myButtonStop.backgroundColor = UIColor.grayColor();
        
        myButtonStart.layer.masksToBounds = true
        myButtonStop.layer.masksToBounds = true
        
        myButtonStart.setTitle("开始", forState: .Normal)
        myButtonStop.setTitle("停止", forState: .Normal)
        
        myButtonStart.layer.cornerRadius = 20.0
        myButtonStop.layer.cornerRadius = 20.0
        
        myButtonStart.layer.position = CGPoint(x: self.view.bounds.width/2 - 70, y:self.view.bounds.height-50)
        myButtonStop.layer.position = CGPoint(x: self.view.bounds.width/2 + 70, y:self.view.bounds.height-50)
        
        myButtonStart.addTarget(self, action: "onClickMyButton:", forControlEvents: .TouchUpInside)
        myButtonStop.addTarget(self, action: "onClickMyButton:", forControlEvents: .TouchUpInside)
        
        self.view.addSubview(myButtonStart);
        self.view.addSubview(myButtonStop);
        
    }
    
    func onClickMyButton(sender: UIButton){
        if( sender == myButtonStart ){
            let paths = NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true)
            let documentsDirectory = paths[0] as String
            let filePath : String? = "\(documentsDirectory)/test.mp4"
            let fileURL : NSURL = NSURL(fileURLWithPath: filePath!)!
            myVideoOutput.startRecordingToOutputFileURL(fileURL, recordingDelegate: self)
        } else if ( sender == myButtonStop ){
            myVideoOutput.stopRecording()
        }
    }
    
    func captureOutput(captureOutput: AVCaptureFileOutput!, didFinishRecordingToOutputFileAtURL outputFileURL: NSURL!, fromConnections connections: [AnyObject]!, error: NSError!) {
        println("didFinishRecordingToOutputFileAtURL")
    }
    
    func captureOutput(captureOutput: AVCaptureFileOutput!, didStartRecordingToOutputFileAtURL fileURL: NSURL!, fromConnections connections: [AnyObject]!) {
        println("didStartRecordingToOutputFileAtURL")
    }
}
{% endcodeblock %}