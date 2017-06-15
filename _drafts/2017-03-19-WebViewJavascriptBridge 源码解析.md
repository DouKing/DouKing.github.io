---
layout: post
title: WebViewJavascriptBridge 源码解析
description: WebViewJavascriptBridge 源码解析
categories: 学习总结
keywords: study
---

## 前言

`WebViewJavascriptBridge` 是 iOS 开发中一个常用的 webView 与 js 交互的三方库，`WebViewJavascriptBridge` 通过一种优雅的方式实现了 native 与 js 互通消息。

webView 与 js 交互的方式大致有一下几种：

1. 拦截协议

2. 使用官方框架 `JavaScriptCore`，从 iOS 7 以后开始支持

3. iOS 8 以后 `WKWebView` 本身自带交互功能

`WebViewJavascriptBridge` 使用的是拦截协议的方式，

Native 注册

```
WebViewJavascriptBridge.m

- (void)registerHandler:(NSString *)handlerName handler:(WVJBHandler)handler {
    _base.messageHandlers[handlerName] = [handler copy];
}

WebViewJavascriptBridgeBase.h

@property (strong, nonatomic) NSMutableDictionary* messageHandlers;
@property (strong, nonatomic) WVJBHandler messageHandler;

```
