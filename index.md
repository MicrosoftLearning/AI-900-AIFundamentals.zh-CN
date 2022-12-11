---
title: 联机托管说明
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Azure AI 基础知识练习

这些动手练习旨在支持 [Microsoft Learn](https://docs.microsoft.com/training/) 上的培训内容。

要完成这些练习，需要有一个 Microsoft Azure 订阅。 可在 [https://azure.microsoft.com](https://azure.microsoft.com) 注册免费试用版。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 练习 |
| ------- | 
{% 表示实验室 % 中的活动}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
