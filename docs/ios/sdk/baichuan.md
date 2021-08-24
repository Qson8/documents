---
title: 百川电商SDK 接入说明及注意点
date: 2019-08-20 19:15:07
tags: 
    - iOS
category:
    - iOS
---

#### 前言

最近在接触阿里百川SDK过程中，发现一些问题觉得有必要通过文档让大家了解下，首先看下阿里百川是什么？
阿里百川开放淘系电商能力，帮助APP开发者在各种场景下快速、低成本搭建无线电商导购业务，开发用户消费需求，实现商业变现。

#### 接入准备

建议阅读下官方文档中的[接入指南](https://baichuan.taobao.com/docs/doc.htm?spm=a3c0d.7629140.0.0.470bbe48GhqZ4k&treeId=129&articleId=104528&docType=1)，有相关介绍这里就不累赘，主要提下重点要注意的。

`安全图片` ： 务必按要求配置好安全图片，并放入项目中。安全图片其实就是将项目签名，包名等信息存储在图片中，编译启动时进行校验，图片名称必须按照规定。
如果有多个项目，务必选择好对应的项目，否则会导致鉴于不匹配情况。

![Snip20190822_1](/image/15664561394389/Snip20190822_1.png)


#### 开始集成SDK
* 安全图片放入项目中，图片名为yw_1222.jpg

* 推荐使用Cocoapod方式接入SDK

  ```
  终端中执行以下命令，添加百川的源
  pod repo add AliBCSpecs http://repo.baichuan-ios.taobao.com/baichuanSDK/AliBCSpecs.git
  ```
  
    ```
  Podfile添加(具体版本以百川开发者网站为准)
  source 'http://repo.baichuan-ios.taobao.com/  baichuanSDK/AliBCSpecs.git'
  pod 'AlibcTradeSDK'
  ```
  
  手动方式引入SDK参考[文档](https://baichuan.taobao.com/docs/doc.htm?spm=a3c0d.7629140.0.0.45f8be48T93N9B&treeId=129&articleId=105648&docType=1),这里就不讲了。

* 配置URL Types

    URL Scheme为tbopen{AppKey},如tbopen123456

    是在阿里百川注册的应用AppKey![1566457415236](/image/15664561394389/1566457415236.jpg)

* Info.plist配置

    ```
    在info.plist中,增加LSApplicationQueriesSchemes字段,并添加tbopen,tmall
    ```
    ![Snip20190822_3](/image/15664561394389/Snip20190822_3.png)

 ```
 配置ATS，允许HTTP请求
 ```
 ![Snip20190822_5](/image/15664561394389/Snip20190822_5.png)

* SDK初始化
    在 AppDelegate 中初始化SDK
    
    ```
    import <AlibcTradeSDK/AlibcTradeSDK.h>
 
    - (BOOL)application:(UIApplication *)application {
        // 百川平台基础SDK初始化，加载并初始化各个业务能力插件
        [[AlibcTradeSDK sharedInstance] asyncInitWithSuccess:^{
             
        } failure:^(NSError *error) {
            NSLog(@"Init failed: %@", error.description);
        }];
         
        // 开发阶段打开日志开关，方便排查错误信息
        //默认调试模式打开日志,release关闭,可以不调用下面的函数
        [[AlibcTradeSDK sharedInstance] setDebugLogOpen:YES];
         
        // 配置全局的淘客参数
        //如果没有阿里妈妈的淘客账号,setTaokeParams函数需要调用
        AlibcTradeTaokeParams *taokeParams = [[AlibcTradeTaokeParams alloc] init];
        taokeParams.pid = @"mm_XXXXX"; //mm_XXXXX为你自己申请的阿里妈妈淘客pid
        [[AlibcTradeSDK sharedInstance] setTaokeParams:taokeParams];
         
        //设置全局的app标识，在电商模块里等同于isv_code
        //没有申请过isv_code的接入方,默认不需要调用该函数
        [[AlibcTradeSDK sharedInstance] setISVCode:@"your_isv_code"];
         
        // 设置全局配置，是否强制使用h5
        [[AlibcTradeSDK sharedInstance] setIsForceH5:NO];
     
        return YES;
    }
    ```
    
* 处理应用跳转

    为了正常使用百川内置的应用跳转处理，需要调用百川SDK的方法。建议优先调用百川处理，如果百川已处理，可以直接返回YES；当然，也可以继续处理，比如记录应用跳转来源日志等。
    以下代码不现实,会导致通过手淘授权登陆,跳回来没反应等问题
    
    ```
    // iOS9以下的实现这个方法，如果需要兼容iOS9以下的话
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation{
        /* 老接口写法 已弃用，建议使用新接口
        if (![[AlibcTradeSDK sharedInstance] handleOpenURL:url]) {
            // 处理其他app跳转到自己的app
        }
        return YES;
        */
     
        // 新接口写法
        if (![[AlibcTradeSDK sharedInstance] application:application 
                                                 openURL:url 
                                       sourceApplication:sourceApplication 
                                              annotation:annotation]) {
            // 处理其他app跳转到自己的app
        }
        return YES;
    ```

    ```
        // iOS9以上的实现这个方法
        - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options {
         
        /* 老接口写法 已弃用，建议使用新接口
        if (![[AlibcTradeSDK sharedInstance] handleOpenURL:url]) {
            // 处理其他app跳转到自己的app
        }
        return YES;
        */
     
        // 新接口写法
        if (![[AlibcTradeSDK sharedInstance] application:application
                                                 openURL:url
                                                 options:options]) {
            //处理其他app跳转到自己的app，如果百川处理过会返回YES
        }
        return YES;} 
    ```
* 方法说明

    ```
    /**
     * 使用isv自己的webview打开page，可以实现淘宝账号免登以及电商交易支付流程
     *
     * @param parentController            webView所在的view controller.
     * @param webView                     isv自己的webview,请先设置好自己的delegate先调用本接口,否则拦截登陆等逻辑会失效
     * @param page                        想要打开的page
     * @param showParams                  打开方式的一些自定义参数
     * @param taoKeParams                 淘客参数
     * @param trackParam                  链路跟踪参数
     * @param tradeProcessSuccessCallback 交易流程中成功回调(加购成功/发生支付)
     * @param tradeProcessFailedCallback  交易流程中退出或者调用发生错误的回调
     *
     * @return  0:  标识跳转到手淘打开了
                1:  标识用h5打开
               -1:  标识出错
     */
    - (NSInteger)           show:(UIViewController * __nonnull)parentController
                         webView:(nullable UIWebView*)webView
                            page:(id<AlibcTradePage> __nonnull)page
                      showParams:(nullable AlibcTradeShowParams*)showParams
                     taoKeParams:(nullable AlibcTradeTaokeParams *)taoKeParams
                      trackParam:(nullable NSDictionary*)trackParam
     tradeProcessSuccessCallback:(nullable void (^)(AlibcTradeResult * __nullable result))onSuccess
      tradeProcessFailedCallback:(nullable void (^)(NSError * __nullable error))onFailure;
    ```
 ```
     showParams 参数
     
     1. 调起手淘
     AlibcTradeShowParams* showParam = [[AlibcTradeShowParams alloc] init];
    showParam.openType = AlibcOpenTypeNative;
    // showParam.backUrl=@"tbopenXXXXX://"; // 官方文档的有问题
    showParam.backUrl=@"tbopenXXXXX"; // 亲测有效
    showParam.isNeedPush=isNeedPush;  
                      
    2. 调起天猫
    AlibcTradeShowParams* showParam = [[AlibcTradeShowParams alloc] init];
    showParam.openType = AlibcOpenTypeNative;
    // showParam.backUrl=@"tbopenXXXXX://"; // 官方文档的有问题
    showParam.backUrl=@"tbopenXXXXX"; // 亲测有效
    showParam.isNeedPush=isNeedPush;
    showParam.linkKey = @"tmall_scheme";//拉起天猫
 ```

    ```
    page参数 page详情页可以通过自己WebView显示，也可以使用SDK定制的
    其中page参数用于指定需要打开的页面,可以使用的页面类型如下表,由AlibcTradePageFactory生成：
    
    //打开SDK定制商品详情页
    id<AlibcTradePage> page = [AlibcTradePageFactory itemDetailPage: @”123456”];
     
    //根据链接打开页面
    id<AlibcTradePage> page = [AlibcTradePageFactory page: @"http://h5.m.taobao.com/cm/snap/index.html?id=527140984722"];
     
    //打开店铺
    id<AlibcTradePage> page = [AlibcTradePageFactory shopPage: @”12333333”];
     
     
    //淘客信息
    AlibcTradeTaokeParams *taoKeParams=[[AlibcTradeTaokeParams alloc] init];
    taoKeParams.pid=nil; //
    //打开方式
    AlibcTradeShowParams* showParam = [[AlibcTradeShowParams alloc] init];
    showParam.openType = AlibcOpenTypeAuto;
     
    [[AlibcTradeSDK sharedInstance].tradeService show: self.navigationController page:page showParams:showParam taoKeParams: nil trackParam: trackParam tradeProcessSuccessCallback:self.onTradeSuccess tradeProcessFailedCallback:self.onTradeFailure];
    ```
    
    ```
    自定义WebView显示详情页
    
    id<AlibcTradePage> page = [AlibcTradePageFactory itemDetailPage: @”123456”];
    //淘客信息
    AlibcTradeTaokeParams *taoKeParams=[[AlibcTradeTaokeParams alloc] init];
    taoKeParams.pid= nil;
    //打开方式
    AlibcTradeShowParams* showParam = [[AlibcTradeShowParams alloc] init];
    showParam.openType = AlibcOpenTypeAuto;
     
     
    // YourWebViewController类中,webview的delegate设置不能放在viewdidload里面,必须在init的时候,否则函数调用的时候还是nil
      YourTradeWebViewController* myView = [[YourTradeWebViewController alloc] init];
       
     
     NSInteger ret = [[AlibcTradeSDK sharedInstance].tradeService show: myView webView: myView.webView page:page showParams:showParam taoKeParams: taoKeParams trackParam:nil tradeProcessSuccessCallback:self.onTradeSuccess tradeProcessFailedCallback:self.onTradeFailure];
     //返回1,说明h5打开,否则不应该展示页面
     if (ret == 1) {
           [self.navigationController pushViewController:view animated:YES];
     }
    ```

#### 注意点：

    
官网文档基本讲的非常清楚，但在开发中才会遇到一点坑，这边在最后给大家讲讲我在对接过程中遇到的问题，帮大家规避，我遇到的问题也花了不少时间解决，阿里的SDK有个毛病，就是客服太难联系上，而且只留个旺旺群，还得下载个旺信，加群，然后群里也没人回复解答，最近自己只能靠自己。
    
* `安全图片`，如果不是你接的SDK，或者未对该图片了解和备注，很可能把图片当成废弃的删了，有朋友就遇到这个问题，删了APP上线无法跳转到手淘，就比较坑了，在这里特别提醒大家一下，建议在项目中用个文件夹装着，引入项目中，文件夹后面加上中文备注。  
![Snip20190822_6](/image/15664561394389/Snip20190822_6.png)


* `backUrl`设置有误，导致跳转到手淘，然后点击返回，回不来App
        
        
![Snip20190822_7](/image/15664561394389/Snip20190822_7.png)
    上面是官方文档的写法，一开始也是对照文档这么配置backUrl，结果就是在手淘点返回一直回不来，期间花了不少时间，重新对文档，查配置情况，一无所获。后来试着改成下面这样，就OK了!
![Snip20190822_9](/image/15664561394389/Snip20190822_9.png)
        

希望这篇文章能给到大家一些帮助，如有疑问欢迎一同来学习交流。QQ群:912759811。

