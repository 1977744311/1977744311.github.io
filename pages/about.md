---
layout: page
title: About
description: 好好学习，天天打码
keywords: Guo ChengZhe, 郭承哲
comments: true
menu: 关于
permalink: /about/
---

我是郭承哲，我想对自己说，你必须非常努力，才可以看起来毫不费力。

人生格言：

接受你必须接受的，改变你能改变的。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
