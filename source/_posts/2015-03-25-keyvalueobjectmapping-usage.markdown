---
layout: post
title: "使用KeyValueObjectMapping"
date: 2015-03-25 13:06:23 +0800
duoshuo_comments: true
comments: true
categories: Objective-C

description: "使用KeyValueObjectMapping"
keywords: KeyValueObjectMapping, 对象映射, Object Mapping
---

>在iOS开发中，经常会用到网络请求。当收到服务器返回的数据后，想从中获取某个值的话，得使用`objectForKey`的方式取。这样感觉不是很方便，而且繁琐。


介绍`KeyValueObjectMapping`对象映射的使用。

下载地址：https://github.com/dchohfi/KeyValueObjectMapping

<!--more-->


>可以在项目使用的地方引用需要的类

	#import "DCKeyValueObjectMapping.h"
	#import "DCParserConfiguration.h"
	#import "DCObjectMapping.h"
	#import "DCArrayMapping.h"



Tweet类


	#import <Foundation/Foundation.h>

	@interface Tweet : NSObject

	@property(nonatomic, strong) NSString *idStr;
	@property(nonatomic, strong) NSString *text;
	@property(nonatomic, strong) NSDate *createdAt;


	@end


	#import "Tweet.h"

	@implementation Tweet

	@end



User类


	#import <Foundation/Foundation.h>

	@interface User : NSObject

	@property(nonatomic, strong) NSString *idStr;
	@property(nonatomic, strong) NSString *name;
	@property(nonatomic, strong) NSString *screenName;
	@property(nonatomic, strong) NSString *location;
	@property(nonatomic, strong) NSString *description1;
	@property(nonatomic, strong) NSURL *url;
	@property(nonatomic) BOOL protected;
	@property(nonatomic, strong) NSDate *createdAt;

	@property(nonatomic, strong) NSArray *tweets;

	@end


	#import "User.h"

	@implementation User

	@end


可以使用下面的方法测试，就会明白其中的使用方法了，哈哈。。。

	//获取本地json数据代码
	NSString *caminhoJson = [[NSBundle bundleForClass: [self class]] pathForResource:@"tweet" ofType:@"json"];
	NSData *data = [NSData dataWithContentsOfFile:caminhoJson];
	NSError *error;
	NSDictionary *jsonParsed = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:&error];



DCKeyValueObjectMapping的使用, tweet.json文件及代码如下

	{
	    "id_str": "27924446",
	    "name": "Diego Chohfi",
	    "screen_name": "dchohfi",
	    "location": "São Paulo",
	    "description1": "Instrutor na @Caelum, desenvolvedor de coração, apaixonado por música e cerveja, sempre cerveja.",
	    "url": "http://about.me/dchohfi",
	    "protected": false,
	    "created_at": "Tue Mar 31 18:01:12 +0000 2009",
	    "tweets": []
	}


	//示例代码:
	DCKeyValueObjectMapping *parser = [DCKeyValueObjectMapping mapperForClass: [Tweet class]];

	Tweet *tweet = [parser parseDictionary:jsonParsed];
	NSLog(@"%@ - %@", tweet.idStr, tweet.name);



DCObjectMapping的使用, tweet.json文件及代码如下

	{
	    "id_str": 190957570511478800,
	    "text": "Tweet text",
	    "user": {
	        "name": "Diego Chohfi",
	        "screen_name": "dchohfi",
	        "location": "São Paulo"
	    }
	}


	//示例代码:
	DCParserConfiguration *config = [DCParserConfiguration configuration];

	DCObjectMapping *textToTweetText = [DCObjectMapping mapKeyPath:@"text" toAttribute:@"tweetText" onClass:[Tweet class]];
	DCObjectMapping *userToUserOwner = [DCObjectMapping mapKeyPath:@"user" toAttribute:@"userOwner" onClass:[Tweet class]];

	[config addObjectMapping:textToTweetText];
	[config addObjectMapping:userToUserOwner];

	DCKeyValueObjectMapping *parser = [DCKeyValueObjectMapping mapperForClass: [Tweet class]  andConfiguration:config];
	Tweet *tweetParsed = [parser parseDictionary:jsonParsed];
	NSLog(@"%@ - %@ - %@ - %@", tweetParsed.idStr, tweetParsed.tweetText, tweetParsed.userOwner.screenName, tweetParsed.userOwner.createdAt);


DCArrayMapping的使用, tweet.json文件及代码如下

	{
	    "id_str": "27924446",
	    "name": "Diego Chohfi",
	    "screen_name": "dchohfi",
	    "location": "São Paulo",
	    "description1": "Instrutor na @Caelum, desenvolvedor de coração, apaixonado por música e cerveja, sempre cerveja.",
	    "url": "http://about.me/dchohfi",
	    "protected": false,
	    "created_at": "Tue Mar 31 18:01:12 +0000 2009",
	    "tweets" : [
	        {
		        "created_at" : "Sat Apr 14 00:20:07 +0000 2012",
		        "id_str" : 190957570511478784,
		        "text" : "Tweet text"
	        },
	        {
		        "created_at" : "Sat Apr 14 00:20:07 +0000 2012",
		        "id_str" : 190957570511478784,
		        "text" : "Tweet text"
	        }
	    ]
	}


	//示例代码:
	DCArrayMapping *mapper = [DCArrayMapping mapperForClassElements:[Tweet class] forAttribute:@"tweets" onClass:[User class]];
	DCParserConfiguration *config = [DCParserConfiguration configuration];
	[config addArrayMapper:mapper];
	DCKeyValueObjectMapping *parser = [DCKeyValueObjectMapping mapperForClass:[User class] andConfiguration:config];
	User *user = [parser parseDictionary:jsonParsed];
	NSLog(@"%@ - %@ - %@", [user.tweets[0] idStr], user.screenName, [user.tweets[0] createdAt]);



