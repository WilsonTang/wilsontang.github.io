---
layout: post
title: "预览画面的效果处理"
date: 2015-02-13 11:04:30 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "预览画面的效果处理"
keywords: UIMotionEffect
---

>代码如下：
<!--more-->
{% codeblock lang:objc%}
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    var myEffectView: UIView!

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }

    func applicationWillResignActive(application: UIApplication) {
        // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
        // Use this method to pause ongoing tasks, disable timers, and throttle down OpenGL ES frame rates. Games should use this method to pause the game.
        
        let effect: UIBlurEffect = UIBlurEffect(style: UIBlurEffectStyle.Light)
        myEffectView = UIVisualEffectView(effect: effect)
        myEffectView.frame = CGRectMake(0, 0, UIScreen.mainScreen().bounds.size.width, UIScreen.mainScreen().bounds.size.height)
        self.window?.addSubview(myEffectView)
    }

    func applicationDidEnterBackground(application: UIApplication) {
        // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
        // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
    }

    func applicationWillEnterForeground(application: UIApplication) {
        // Called as part of the transition from the background to the inactive state; here you can undo many of the changes made on entering the background.
    }

    func applicationDidBecomeActive(application: UIApplication) {
        // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
        
        if myEffectView != nil {
            self.myEffectView.removeFromSuperview()
        }
    }

    func applicationWillTerminate(application: UIApplication) {
        // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
    }
}

//ViewController
import UIKit

class ViewController: UIViewController, UIWebViewDelegate {

    var myWebView : UIWebView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        myWebView = UIWebView()
        myWebView.delegate = self
        myWebView.frame = self.view.bounds
        self.view.addSubview(myWebView)
        
        let url: NSURL! = NSURL(string: "http://www.apple.com")
        let request: NSURLRequest = NSURLRequest(URL: url)
        myWebView.loadRequest(request)
        
    }
    
    func webViewDidFinishLoad(webView: UIWebView!) {
        println("webViewDidFinishLoad")
    }
    
    func webViewDidStartLoad(webView: UIWebView!) {
        println("webViewDidStartLoad")
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
{% endcodeblock %}