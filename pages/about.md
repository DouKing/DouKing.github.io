---
layout: page
title: About
description:
keywords:
comments: true
menu: 关于
permalink: /about/
---

来自北京的iOS开发者，努力工作，努力学习中。

追求「简洁而优雅的哲学」。

## 联系

* GitHub：[@Douking](https://github.com/douking)
* 博客：[{{ site.title }}]({{ site.url }})

## Skill Keywords

#### Software Engineer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_software_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### Mobile Developer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_mobile_app_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>
