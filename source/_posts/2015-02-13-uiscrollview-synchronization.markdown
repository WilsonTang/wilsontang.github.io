---
layout: post
title: "UIScrollView同步"
date: 2015-02-13 11:18:36 +0800
duoshuo_comments: true
comments: true
categories: Swift

description: "UIScrollView同步"
keywords: UIScrollView, UIPageControl, uiscrollView synchronization
---

>效果图

{% img /images/blogImgs/uiScrollView_1.png 320 568 %}
{% img /images/blogImgs/uiScrollView_2.png 320 568 %}

>代码如下：

<!--more-->
{% codeblock lang:objc%}
import UIKit

class ViewController: UIViewController, UIScrollViewDelegate {
    var pageControl: UIPageControl!
    var scrollViewHeader: UIScrollView!
    var scrollViewMain: UIScrollView!
    let pageSize = 10
    
    override func viewDidLoad() {
        
        let width = self.view.frame.maxX, height = self.view.frame.maxY
        
        scrollViewHeader = UIScrollView(frame: self.view.frame)
        scrollViewHeader.showsHorizontalScrollIndicator = false
        scrollViewHeader.showsVerticalScrollIndicator = false
        scrollViewHeader.pagingEnabled = true
        scrollViewHeader.delegate = self
        scrollViewHeader.contentSize = CGSizeMake(CGFloat(pageSize) * width, 0)
        self.view.addSubview(scrollViewHeader)
        
        scrollViewMain = UIScrollView(frame: self.view.frame)
        scrollViewMain.showsHorizontalScrollIndicator = false
        scrollViewMain.showsVerticalScrollIndicator = false
        scrollViewMain.pagingEnabled = true
        scrollViewMain.delegate = self
        scrollViewMain.contentSize = CGSizeMake(CGFloat(pageSize) * width, 0)
        self.view.addSubview(scrollViewMain)
        
        for var i = 0; i < pageSize; i++ {
            let myLabel:UILabel = UILabel(frame: CGRectMake(CGFloat(i) * width + width/2 - 40, height/2 - 40, 80, 80))
            myLabel.backgroundColor = UIColor.blackColor()
            myLabel.textColor = UIColor.whiteColor()
            myLabel.textAlignment = NSTextAlignment.Center
            myLabel.layer.masksToBounds = true
            myLabel.text = "Page\(i)"
            myLabel.font = UIFont.systemFontOfSize(UIFont.smallSystemFontSize())
            myLabel.layer.cornerRadius = 40.0
            
            scrollViewMain.addSubview(myLabel)
        }
        
        for var i = 0; i < pageSize; i++ {
            let myLabel:UILabel = UILabel(frame: CGRectMake(CGFloat(i) * width/4 + width/2 - 40, 50, 80, 60))
            myLabel.backgroundColor = UIColor.redColor()
            myLabel.textColor = UIColor.whiteColor()
            myLabel.textAlignment = NSTextAlignment.Center
            myLabel.layer.masksToBounds = true
            myLabel.text = "Page\(i)"
            myLabel.font = UIFont.systemFontOfSize(UIFont.smallSystemFontSize())
            myLabel.layer.cornerRadius = 30.0
            
            scrollViewHeader.addSubview(myLabel)
        }
        
        pageControl = UIPageControl(frame: CGRectMake(0, self.view.frame.maxY - 50, width, 50))
        pageControl.backgroundColor = UIColor.lightGrayColor()
        
        pageControl.numberOfPages = pageSize
        
        pageControl.currentPage = 0
        pageControl.userInteractionEnabled = false
        
        self.view.addSubview(pageControl)
    }

    func scrollViewDidScroll(scrollView: UIScrollView) {
        if scrollView == scrollViewMain {
            scrollViewHeader.contentOffset.x = scrollViewMain.contentOffset.x/4
        }
    }
    
    func scrollViewDidEndDecelerating(scrollView: UIScrollView) {
        if fmod(scrollViewMain.contentOffset.x, scrollViewMain.frame.maxX) == 0 {
            pageControl.currentPage = Int(scrollViewMain.contentOffset.x / scrollViewMain.frame.maxX)
        }
    }
}
{% endcodeblock %}