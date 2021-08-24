---
title: iPhone手机型号信息大全
date: 2017-12-01 18:13:40
tags: 
    - iOS
category:
    - iOS
---


设备型号官网地址：https://www.theiphonewiki.com/wiki/Models

2018年9月新增设备
`
iPhone XR , iPhone XS, iPhone XS Max
`



```objc
//获得设备型号
+ (NSString *)getCurrentDevice
{
    int mib[2];
    size_t len;
    char *machine;
    
    mib[0] = CTL_HW;
    mib[1] = HW_MACHINE;
    sysctl(mib, 2, NULL, &len, NULL, 0);
    machine = malloc(len);
    sysctl(mib, 2, machine, &len, NULL, 0);
    
    NSString *platform = [NSString stringWithCString:machine encoding:NSASCIIStringEncoding];
    free(machine);

    // iPhone
    if ([platform isEqualToString:@"iPhone1,1"]) return @"iPhone2G";
    if ([platform isEqualToString:@"iPhone1,2"]) return @"iPhone3G";
    if ([platform isEqualToString:@"iPhone2,1"]) return @"iPhone3GS";
    if ([platform isEqualToString:@"iPhone3,1"]) return @"iPhone4";
    if ([platform isEqualToString:@"iPhone3,2"]) return @"iPhone4";
    if ([platform isEqualToString:@"iPhone3,3"]) return @"iPhone4";
    if ([platform isEqualToString:@"iPhone4,1"]) return @"iPhone4S";
    if ([platform isEqualToString:@"iPhone5,1"]) return @"iPhone5";
    if ([platform isEqualToString:@"iPhone5,2"]) return @"iPhone5";
    if ([platform isEqualToString:@"iPhone5,3"]) return @"iPhone5c";
    if ([platform isEqualToString:@"iPhone5,4"]) return @"iPhone5c";
    if ([platform isEqualToString:@"iPhone6,1"]) return @"iPhone5s";
    if ([platform isEqualToString:@"iPhone6,2"]) return @"iPhone5s";
    if ([platform isEqualToString:@"iPhone7,2"]) return @"iPhone6";
    if ([platform isEqualToString:@"iPhone7,1"]) return @"iPhone6Plus";
    if ([platform isEqualToString:@"iPhone8,1"]) return @"iPhone6s";
    if ([platform isEqualToString:@"iPhone8,2"]) return @"iPhone6sPlus";
    if ([platform isEqualToString:@"iPhone8,3"]) return @"iPhoneSE";
    if ([platform isEqualToString:@"iPhone8,4"]) return @"iPhoneSE";
    if ([platform isEqualToString:@"iPhone9,1"]) return @"iPhone7";
    if ([platform isEqualToString:@"iPhone9,2"] ||
        [platform isEqualToString:@"iPhone9,4"]) return @"iPhone7Plus";
    if ([platform isEqualToString:@"iPhone10,1"] ||
        [platform isEqualToString:@"iPhone10,4"]) return @"iPhone 8";
    if ([platform isEqualToString:@"iPhone10,2"] ||
        [platform isEqualToString:@"iPhone10,5"]) return @"iPhone 8 Plus";
    if ([platform isEqualToString:@"iPhone10,3"] ||
        [platform isEqualToString:@"iPhone10,6"]) return @"iPhone X";
    if ([platform isEqualToString:@"iPhone11,8"]) return @"iPhone XR";
    if ([platform isEqualToString:@"iPhone11,2"]) return @"iPhone XS";
    if ([platform isEqualToString:@"iPhone11,4"] ||
        [platform isEqualToString:@"iPhone11,6"]) return @"iPhone XS Max";
    
    //iPod Touch
    if ([platform isEqualToString:@"iPod1,1"])   return @"iPodTouch";
    if ([platform isEqualToString:@"iPod2,1"])   return @"iPodTouch2G";
    if ([platform isEqualToString:@"iPod3,1"])   return @"iPodTouch3G";
    if ([platform isEqualToString:@"iPod4,1"])   return @"iPodTouch4G";
    if ([platform isEqualToString:@"iPod5,1"])   return @"iPodTouch5G";
    if ([platform isEqualToString:@"iPod7,1"])   return @"iPodTouch6G";
    
    //iPad
    if ([platform isEqualToString:@"iPad1,1"])   return @"iPad";
    if ([platform isEqualToString:@"iPad2,1"])   return @"iPad2";
    if ([platform isEqualToString:@"iPad2,2"])   return @"iPad2";
    if ([platform isEqualToString:@"iPad2,3"])   return @"iPad2";
    if ([platform isEqualToString:@"iPad2,4"])   return @"iPad2";
    if ([platform isEqualToString:@"iPad3,1"])   return @"iPad3";
    if ([platform isEqualToString:@"iPad3,2"])   return @"iPad3";
    if ([platform isEqualToString:@"iPad3,3"])   return @"iPad3";
    if ([platform isEqualToString:@"iPad3,4"])   return @"iPad4";
    if ([platform isEqualToString:@"iPad3,5"])   return @"iPad4";
    if ([platform isEqualToString:@"iPad3,6"])   return @"iPad4";
    
    //iPad Air
    if ([platform isEqualToString:@"iPad4,1"])   return @"iPadAir";
    if ([platform isEqualToString:@"iPad4,2"])   return @"iPadAir";
    if ([platform isEqualToString:@"iPad4,3"])   return @"iPadAir";
    if ([platform isEqualToString:@"iPad5,3"])   return @"iPadAir2";
    if ([platform isEqualToString:@"iPad5,4"])   return @"iPadAir2";
    
    //iPad pro
    if ([platform isEqualToString:@"iPad6,3"])   return @"iPadPro";
    if ([platform isEqualToString:@"iPad6,4"])   return @"iPadPro";
    if ([platform isEqualToString:@"iPad6,7"])   return @"iPadPro";
    if ([platform isEqualToString:@"iPad6,8"])   return @"iPadPro";
    if ([platform isEqualToString:@"iPad6,11"] ||
        [platform isEqualToString:@"iPad6,12"]) return @"iPad 5";
    if ([platform isEqualToString:@"iPad7,1"] ||
        [platform isEqualToString:@"iPad7,2"]) return @"iPad Pro 12.9-inch 2";
    if ([platform isEqualToString:@"iPad7,3"] ||
        [platform isEqualToString:@"iPad7,4"]) return @"iPad Pro 10.5-inch";
    
    //iPad mini
    if ([platform isEqualToString:@"iPad2,5"])   return @"iPadmini1G";
    if ([platform isEqualToString:@"iPad2,6"])   return @"iPadmini1G";
    if ([platform isEqualToString:@"iPad2,7"])   return @"iPadmini1G";
    if ([platform isEqualToString:@"iPad4,4"])   return @"iPadmini2";
    if ([platform isEqualToString:@"iPad4,5"])   return @"iPadmini2";
    if ([platform isEqualToString:@"iPad4,6"])   return @"iPadmini2";
    if ([platform isEqualToString:@"iPad4,7"])   return @"iPadmini3";
    if ([platform isEqualToString:@"iPad4,8"])   return @"iPadmini3";
    if ([platform isEqualToString:@"iPad4,9"])   return @"iPadmini3";
    if ([platform isEqualToString:@"iPad5,1"])   return @"iPadmini4";
    if ([platform isEqualToString:@"iPad5,2"])   return @"iPadmini4";
    
    if ([platform isEqualToString:@"i386"])      return @"iPhoneSimulator";
    if ([platform isEqualToString:@"x86_64"])    return @"iPhoneSimulator";
    return @"Unknown";
}
```


#### iPhone:

| 机型 | 像素 | 比例 | ppi | 尺寸 | 机型代码 | 发布时间 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| iPhone XR | 1792×828 | 19.5:9 | 326 | 6.1 | iPhone11,8 | 2018.09 |
| iPhone XS Max | 2688×1242 | 18:9 | 458 | 6.5 | iPhone11,4 iPhone11,6 | 2018.09 |
| iPhone XS | 2436×1125 | 18:9 | 458 | 5.8 | iPhone11,2 | 2018.09 |
| iPhone X | 2436×1125 | 18:9 | 458 | 5.8 | iPhone10,3 iPhone10,6 | 2017.09 |
| iPhone 8 plus | 1920×1080 | 16:9 | 401 | 5.5 | iPhone10,2 iPhone10,5 | 2017.09 |
| iPhone 8 | 1334×750 | 16:9 | 401 | 4.7 | iPhone10,1 iPhone10,4 | 2017.09 |
| iPhone 7 plus | 1920×1080 | 16:9 | 401 | 5.5 | iPhone9,2 iPhone9,4 | 2016.09 |
| iPhone 7 | 1334×750 | 16:9 | 401 | 4.7 | iPhone9,1 iPhone9,3| 2016.09 |
| iPhone 5 SE | 1136×640 | 16:9 | 401 | 4.0 | iPhone8,4 | 2016.03 |
| iPhone 6s plus | 1920×1080 | 16:9 | 401 | 5.5 | iPhone8,1 | 2015.09 |
| iPhone 6s | 1334×750 | 16:9 | 401 | 4.7 | iPhone8,2 | 2015.09 |
| iPhone 6 plus | 1920×1080 | 16:9 | 401 | 5.5 | iPhone7,1 | 2014.09 |
| iPhone 6 | 1334×750 | 16:9 | 401 | 4.7 | iPhone7,2 | 2014.09 |
| iPhone 5s | 1136×640 | 16:9 | 326 | 4.0 | iPhone6,1 iPhone6,2 | 2013.09 |



#### iPad:


| 机型 | 逻辑分辨率 | Scale | 物理分辨率 | 比例 | ppi | 尺寸 | 型号代码 | 发布时间 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| iPad 5 | 1024×768 | @2x | 2048×1536 | 4:3 | 264 | 9.7 | iPad6,11 iPad6,12 | 2017.03 |
| iPad 4 | 1024×768 | @2x | 2048×1536 | 4:3 | 264 | 9.7 | iPad3,4 iPad3,5 iPad3,6 | 2012.10 |
| iPad 3 | 1024×768 | @2x | 2048×1536 | 4:3 | 264 | 9.7 | iPad3,1 iPad3,2 iPad3,3 | 2012.03 |
| iPad 2 | 1024×768 | @1x | 1024×768 | 4:3 | 163 | 9.7 | iPad2,1 iPad2,2 iPad2,3 iPad2,4 | 2011.03 |
| iPad | 1024×768 | @1x | 1024×768 | 4:3 | 163 | 9.7 | iPad1,1 | 2010.01 |

#### iPad Air:

| 机型 | 逻辑分辨率 | Scale | 物理分辨率 | 比例 | ppi | 尺寸 | 型号代码 | 发布时间 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| iPad Air | 1024×768 | @2x | 2048×1536 | 4:3 | 264 | 9.7 | iPad4,1 iPad4,2 iPad4,3 | 2013.10 |
| iPad Air 2 | 1024×768 | @2x | 2048×1536 | 4:3 | 264 | 9.7 | iPad5,3 iPad5,4 | 2014.10 |


#### iPad Pro:

| 机型 | 逻辑分辨率 | Scale | 物理分辨率 | 比例 | ppi | 尺寸 | 型号代码 | 发布时间 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| iPad Pro 10.5 | 1112×834 | @2x | 2224×1668 | 4:3 | 264 | 10.5 | iPad7,3 iPad7,4 | 2017 |
| iPad Pro 12.9-inch 2 | 1366×1024 | @2x | 2732×2048 | 4:3 | 264 | 12.9 | iPad7,1 iPad7,2 | 2017 |
| iPad Pro 9.7-inch | 1024×768 | @2x | 2048×1536 | 4:3 | 264 | 9.7 | iPad6,3 iPad6,4 | 2016.03 |
| iPad Pro 12.9-inch | 1366×1024 | @2x | 2732×2048 | 4:3 | 264 | 12.9 | iPad6,7 iPad6,8 | 2015.09 |

#### iPad Mini:

| 机型 | 逻辑分辨率 | Scale | 物理分辨率 | 比例 | ppi | 尺寸 | 型号代码 | 发布时间 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| iPad mini 4 | 1024×768 | @2X | 2048×1536 | 4:3 | 326 | 7.9 | iPad5,1 iPad5,2 iPad4,9 | 2015.09 |
| iPad mini 3 | 1024×768 | @2X | 2048×1536 | 4:3 | 326 | 7.9 | iPad4,7 iPad4,8 iPad4,9 | 2014.10 |
| iPad mini 2 | 1024×768 | @2X | 2048×1536 | 4:3 | 326 | 7.9 | iPad4,5 iPad4,6 iPad4,7 | 2013.10 |
| iPad mini | 1024×768 | @1X | 1024×768 | 4:3 | 163 | 7.9 | iPad2,5 iPad2,6 iPad2,7 | 2012.10 |

#### Samulitor:

| 机型 | 型号代码 | 
| :-: | :-: |
| Simulator | i386、x86_64 |
| Unknown |  |

