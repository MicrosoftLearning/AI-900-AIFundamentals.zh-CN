---
title: 联机托管说明
permalink: index.html
layout: home
---

# Azure AI 基础知识练习

这些动手练习旨在支持 [Microsoft Learn](https://docs.microsoft.com/training/) 上的培训内容。

要完成这些练习，需要有一个 Microsoft Azure 订阅。 可在 [https://azure.microsoft.com](https://azure.microsoft.com) 注册免费试用版。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %}
| 练习 |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
