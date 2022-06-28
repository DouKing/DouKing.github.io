---
layout: post
title: Cocoapods 安装 subpods 时使用 ssh 代替 https
description: Cocoapods 安装 subpods 时使用 ssh 代替 https
categories: 学习总结
keywords: cocoapods, ssh
---

使用 cocoapods 安装 GitHub 上的第三方库时，默认会使用 https 方式安装。如果想强制使用 ssh 方式安装，可将下面内容粘贴到 ~/.gitconfig 文件中

```
[url "git@github.com:"]
  insteadOf = https://github.com/
[url "git@github.com:"]
  pushInsteadOf = "git://github.com/"
[url "git@github.com:"]
  pushInsteadOf = "https://github.com/"
```