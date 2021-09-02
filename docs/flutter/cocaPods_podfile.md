---
title: 混合开发CocoaPods Podfile配置
date: 2019-12-09 10:17:27
tags: 
    - flutter
category:
    - flutter
---

# CocoaPods Podfile配置

> 官方推荐混合开发使用 CocoaPods 依赖管理和已安装的 Flutter SDK，则在 Xcode 中编译应用，就可以自动运行脚本来集成 dart 代码和 plugin。当然在此之前在原生iOS工程中集成Flutter需要先配置好CocoaPods，[CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html)是iOS的类库管理工具，用来管理第三方开源库。在原生iOS工程中执行pod init命令创建一个Podfile文件，然后在Podfile文件中添加Flutter模块依赖。

## Podfile解析

### 源码
Podfile是采用Ruby语法，编写的脚本文件，flutter_boost官方demo中内容如下

```ruby
# Uncomment this line to define a global platform for your project
# platform :ios, '9.0'

# CocoaPods analytics sends network stats synchronously affecting flutter build latency.
ENV['COCOAPODS_DISABLE_STATS'] = 'true'

project 'Runner', {
  'Debug' => :debug,
  'Profile' => :release,
  'Release' => :release,
}

def flutter_root
  generated_xcode_build_settings_path = File.expand_path(File.join('..', 'Flutter', 'Generated.xcconfig'), __FILE__)
  unless File.exist?(generated_xcode_build_settings_path)
    raise "#{generated_xcode_build_settings_path} must exist. If you're running pod install manually, make sure flutter pub get is executed first"
  end

  File.foreach(generated_xcode_build_settings_path) do |line|
    matches = line.match(/FLUTTER_ROOT\=(.*)/)
    return matches[1].strip if matches
  end
  raise "FLUTTER_ROOT not found in #{generated_xcode_build_settings_path}. Try deleting Generated.xcconfig, then run flutter pub get"
end

require File.expand_path(File.join('packages', 'flutter_tools', 'bin', 'podhelper'), flutter_root)

flutter_ios_podfile_setup

target 'Runner' do
  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
  
  pod 'AFNetworking'
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
  end
end

```

### 名词解析：
* `File.expand_path` :当前目录的父路径
* `File.joi` :拼接，并使用/分割 如 ../Flutter/Generated.xcconfig
* `File.exist` :路径是否存在，返回值为bool类型
* `unless` :unless式和 if式作用相反,后面语句为假则执行
* `raise` :抛异常
* `File.foreach do |line|` :遍历目录文件并逐行读取文件
* `line.match` :正则匹配
* `require` :引入文件
* `def` :定义方法

