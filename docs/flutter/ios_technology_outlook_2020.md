---
title: 2020年苹果审核新规及Flutter跨平台技术展望
---

<img :src="$withBase('/flutter/apple_technology_outlook_2020/1575903189341.jpg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">


#### 审核新规
我们都知道苹果公司对应用的审核是最系统最严格的，不管是大厂还是小企业项目，都难逃这关。但马上在明天3月份，苹果终于对h5这类App痛下杀手。

<img :src="$withBase('/flutter/apple_technology_outlook_2020/WechatIMG121.jpeg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">

这是一份苹果在9月份发布的消息，给6个月的时间，让通知涉及的即将违规的应用尽快升级优化。也就是在2020年3月份，苹果将全面下架基于H5技术开发的APP，让webapp,hydrid混合栈开发前景堪忧，夹缝中生存无望。苹果审核最近动作频频，从审核情况到每日下架的应用中不难发现，以往活跃在特殊领域的App，越来越难以过审，加之企业版签名的应用掉签率非常高，更有最近个人开发账号申请付费之难，以及各大社交软件高价收购个人账号的消息频频发出，看来生存在苹果边缘领域的项目越来越难存活了，这将导致一部分相关人员被迫失业，继而重新找工作。

<img :src="$withBase('/flutter/apple_technology_outlook_2020/WechatIMG1085.jpeg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">


#### 何为H5 App
H5 App就是依托原生为壳,通过webView显示web服务部署的H5页面，这个页面苹果是无法审核把控。苹果审核规范有规定，如果一个App大部分都是通过WebView打开在线URL地址，那么苹果不建议我们以App的形式提交审核，而会让你用safari打开，说的直白点就是过不了审核。以往还能通过隐藏开关的形式来规避这一问题，但3个月后可能就很难侥幸逃过苹果的审核新规了，当然即使再严的规则总是会有人研究如何攻破，结果如何暂时还不得而知，答案只能交给专研审核这块的技术大牛了。

包括想通过h5技术实现热更新，也会受到影响，我们知道H5可以不经过苹果提交版本和审核,直接动态更新页面内容，至于在什么时候，显示什么内容，苹果公司很难监控，但每天还是能审查出很多违规App。那种太过火或者无视规则的应用肯定是容易被筛选出来。

项目中单纯的使用webView显示文章，不涉及违规的话上架是不受影响，例如新闻类App，正文基本采用webView加载html的方法显示，所以常规应用我们不必担心，其他能尽量使用原生开发的还是乖乖的照做，谁叫iOS系统就苹果爸爸说了算呢。

#### 原生春天到来
H5 APP因其他开发周期短，更新方便快捷，深受很多中小企业项目的青睐，我见过很多项目就一个原生的vc，里面一个WebView，然后就是加载H5，成了一个App，纯粹的H5 App，对于企业成本非常低，虽然说体验不如原生，但在项目初期，还是有很多会这么干。苹果此举针对这类App禁止上架，势必导致项目成本人力成本加大，周期变长，可能对一些企业影响会非常大。但对于苹果而已，流程的系统，希望给用户带来的是体验很好的应用，长期来看，苹果此举也是在净化生态系统，还用户良好的体验感。

#### Flutter新技术
<img :src="$withBase('/flutter/apple_technology_outlook_2020/timg.jpg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">

Flutter是谷歌的移动UI框架，可以快速在iOS和Android上构建高质量的原生用户界面。 Flutter可以与现有的代码一起工作。在全世界，Flutter正在被越来越多的开发者和组织使用，并且Flutter是完全免费、开源的。

刚刚说到原生，现在提起Flutter可能有人有疑惑，Flutter其实就是基于原生开发出来的一个前端框架，他封装了安卓和iOS两个平台的库，使用Dart语言可实现快速开发两端App，而并不是基于H5技术。

<img :src="$withBase('/flutter/apple_technology_outlook_2020/1575903361024.jpg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">

目前已经有非常多的大中小型项目在使用这项技术，国内大厂有闲鱼、今日 头条，腾讯、快手等，不知名的小项目纯Flutter开发的应用也非常多，还不包括国外。而且就今年各社交技术群，论坛对Flutter的讨论也是非常之多，我们来看一组数据，如下图github,Flutter仓库有75K记录，start有81K，Flutter 1.0正式版出来才1年多点，数据接近React Native了。

<img :src="$withBase('/flutter/apple_technology_outlook_2020/1575901319628.jpg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">
<img :src="$withBase('/flutter/apple_technology_outlook_2020/1575901388825.jpg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">

Flutter相关插件也越来越多，学习资料也非常多，感兴趣的都可以在各大网站找到教程学习，这里推荐https://flutterchina.club 或书籍《Flutter实战》来了解学习，本人公众号发布和准备了相关[技术文章](https://mp.weixin.qq.com/s?__biz=MzIyMjQ5NTI4Ng==&tempkey=MTAzOF9hckRtV3Zseld0cnBjNVhuVGhScFdSMmVQSUgwZk1US0d2aWxWMEhkU1YtT3ZEMGxKZEk2ZG1vZl9GUF9MNzdBajY2a3RmcnBvTlBDU1BwajBVOFZsU2pvMmdQdHE5VnNyYm12dmdLRjFSeVhaVHZJM003eGZEYUp6djlqeFQxVDBJRWotUWJLZ2Q2N0hZcjFRcnhSSUF6Yk9ndEV3S0NxbVBFdldBfn4%3D&chksm=682dd6395f5a5f2f3c2acd51b0565b885299c974ff57aa213bdf735bfc7a10dd0f2dd657a3c1#rd)，可供大家一起交流和学习

<img :src="$withBase('/flutter/apple_technology_outlook_2020/1575903608840.jpg')" alt="https://img01.jituwang.com/171030/256786-1G030214S965.jpg">


### 文末总结
作为一名的iOS开发工程师，对应用的体验和系统的流程也非常挑剔，还是挺理解苹果的做法，毕竟苹果花大成本在语言，底层框架，生态上，每个版本都在优化升级系统。虽然难免系统出现bug，但还是可以及时通过升级来解决，来实现流程的系统，这也是我一直使用苹果手机，从未换安卓机器的缘由。2020年，跨平台开发也是个热点，而具有原生特性的Flutter作为新晋技术会越来越普及，相信明年Flutter会更成熟，更稳定。

