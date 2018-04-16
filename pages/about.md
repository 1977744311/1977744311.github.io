---
layout: page
title: About
description: 好好学习，天天打码
keywords: Guo ChengZhe, 郭承哲
comments: true
menu: 关于
permalink: /about/
---

我是郭承哲，目前想改掉自己不写博客的臭毛病，立志从四月16号起，一天一篇博客，写一些每天的收获和感想。

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
