---
title: 联机托管说明
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Azure AI 基础知识练习

这些动手练习旨在支持 [Microsoft Learn](https://docs.microsoft.com/training/) 上的培训内容。

To complete these exercises, you'll need a Microsoft Azure subscription. You can sign up for a free trial at <bpt id="p1">[</bpt><ph id="ph1">https://azure.microsoft.com</ph><ept id="p1">](https://azure.microsoft.com)</ept>.

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 练习 |
| ------- | 
{% 表示实验室 % 中的活动}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
