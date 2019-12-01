---
layout: post
title: iOS13 webView 陀螺仪失效
description: 
categories: 学习总结
keywords: webview,陀螺仪
---

在 iOS13 以前，webview 是可以直接调用陀螺仪的，但是从 iOS13 开始，webview 不能直接使用陀螺仪。

若要 webview 支持陀螺仪，需要：

1.手动调用 `DeviceOrientationEvent.requestPermission()`。注意不能在页面加载时自动调用，需要用户进行操作（比如点击按钮）。这会弹出一个权限对话框，来让用户选择是否允许访问。

2.网站需要支持 HTTPs。