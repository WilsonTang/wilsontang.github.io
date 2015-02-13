---
layout: post
title: "QR码阅读器"
date: 2015-02-13 11:48:59 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "QR码阅读器"
keywords: AVFoundation, AVCaptureMetadataOutputObjectsDelegate, AVCaptureSession, AVCaptureDevice, AVCaptureDevicePosition, AVCaptureDeviceInput, AVCaptureMetadataOutput, AVCaptureVideoPreviewLayer
---

>效果图

{% img /images/blogImgs/avFoundation_3.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit
import AVFoundation

class ViewController: UIViewController, AVCaptureMetadataOutputObjectsDelegate {
    let mySession: AVCaptureSession! = AVCaptureSession()
    let devices = AVCaptureDevice.devices()
    var myDevice: AVCaptureDevice!
    var qrCodeFrameView:UIView!
    var myVideoLayer : AVCaptureVideoPreviewLayer!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        for device in devices{
            if(device.position == AVCaptureDevicePosition.Back){
                myDevice = device as AVCaptureDevice
            }
        }
        
        let myVideoInput = AVCaptureDeviceInput.deviceInputWithDevice(myDevice, error: nil) as AVCaptureDeviceInput
        
        if mySession.canAddInput(myVideoInput) {
            mySession.addInput(myVideoInput)
        }
        
        let myMetadataOutput: AVCaptureMetadataOutput! = AVCaptureMetadataOutput()
        
        if mySession.canAddOutput(myMetadataOutput) {
            mySession.addOutput(myMetadataOutput)
            myMetadataOutput.setMetadataObjectsDelegate(self, queue: dispatch_get_main_queue())
            myMetadataOutput.metadataObjectTypes = [AVMetadataObjectTypeQRCode]
        }
        
        myVideoLayer = AVCaptureVideoPreviewLayer.layerWithSession(mySession) as AVCaptureVideoPreviewLayer
        myVideoLayer.frame = self.view.bounds
        myVideoLayer.videoGravity = AVLayerVideoGravityResizeAspectFill
        
        self.view.layer.addSublayer(myVideoLayer)
        
        qrCodeFrameView = UIView()
        qrCodeFrameView.layer.borderColor = UIColor.greenColor().CGColor
        qrCodeFrameView.layer.borderWidth = 2
        self.view.addSubview(qrCodeFrameView)
        self.view.bringSubviewToFront(qrCodeFrameView)
        
        mySession.startRunning()
    }
    
    func captureOutput(captureOutput: AVCaptureOutput!, didOutputMetadataObjects metadataObjects: [AnyObject]!, fromConnection connection: AVCaptureConnection!) {
        if metadataObjects.count > 0 {
            let qrData: AVMetadataMachineReadableCodeObject  = metadataObjects[0] as AVMetadataMachineReadableCodeObject
            let barCodeObject = myVideoLayer.transformedMetadataObjectForMetadataObject(qrData as AVMetadataMachineReadableCodeObject) as AVMetadataMachineReadableCodeObject
            
            self.qrCodeFrameView.frame = barCodeObject.bounds
//            UIApplication.sharedApplication().openURL(NSURL(string: qrData.stringValue)!)
        } else {
            self.qrCodeFrameView.frame = CGRectZero
        }
    }
}
{% endcodeblock %}