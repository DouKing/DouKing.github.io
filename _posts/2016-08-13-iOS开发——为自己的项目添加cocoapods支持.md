---
layout: post
title: iOS 开发——为自己的项目添加 cocoapods 支持
description: iOS 开发——为自己的项目添加 cocoapods 支持
categories: 学习总结
keywords: git, cocoapods, podspec
---


#### 1.创建项目

首先创建一个项目，并上传至 github 仓库，license文件我们选择 `MIT`

#### 2.为项目创建 podspec 文件

切换到项目根目录，运行下面的命令

> pod spec create [Sample]

这时会在项目根目录生成 `Sample.podspec` 文件

编辑 `Sample.podspec`

```

Pod::Spec.new do |s|

  s.name         = "Sample"
  s.version      = "0.0.1"
  s.summary      = "Cocoapods例子"
  s.homepage     = "https://github.com/DouKing/Sample"
  s.license      = "MIT"
  s.author       = { "wuyikai" => "wuyikai@secoo.com" }
  s.platform     = :ios, "7.0"
  s.source       = { :git => "https://github.com/DouKing/Sample.git", :tag => "#{s.version}" }
  s.source_files = "Sample/**/*.{h,m}"
  s.requires_arc = true

end

```

这里需要注意 `s.version` 要和 `s.source` 里面 `tag` 的版本号要一致，不然后面验证会不通过

验证 podspec 文件，运行下面的命令

> pod lib lint

如果不想要警告：

> pod lib lint --allow-warnings

如果想让错误信息更丰富：

> pod lib lint --verbose

#### 3.打tag上传

验证通过后，为项目打上tag，并推到远端

> git tag xxx

> git push --tags

使用 `trunk` 命令，把 podspec 文件推送到 CocoaPod 官方库

> pod trunk push [Sample.podspec]

#### 4.使用

如果一切顺利，使用 `pod search Sample` 便可以搜索到。

完成！

**注意**：

- `trunk` 命令需要注册，参考[Cocoapods官方网站](https://guides.cocoapods.org/making/getting-setup-with-trunk.html)
- 在打 tag 时要注意包含podspec文件里的version

### Cocoapods私有仓库相关命令

1.Cocoapods 仓库列表

> pod repo list

2.将 git 地址添加到 Cocoapods 仓库

> pod repo add specs [url]

3.将 podspec 文件上传到 Cocoapods 仓库

> pod repo push [仓库名] [xxx.podspec] --allow-warnings --use-libraries [--sources=https://xxxxxxxx]
