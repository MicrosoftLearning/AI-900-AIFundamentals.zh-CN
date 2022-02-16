---
title: 在线托管说明
permalink: index.html
layout: home
---

# Azure AI 基础知识练习

此存储库包含针对 Microsoft 课程 [AI-900 *Microsoft Azure 人工智能*](https://docs.microsoft.com/zh-cn/learn/certifications/courses/ai-900t00)以及 Microsoft Learn 上的等效自定进度模块的动手实验室练习：[Azure 上的人工智能入门](https://docs.microsoft.com/learn/paths/get-started-with-artificial-intelligence-on-azure/)、[使用 Azure 机器学习创建无代码预测模型](https://docs.microsoft.com/zh-cn/learn/paths/create-no-code-predictive-models-azure-machine-learning/)、[探索 Microsoft Azure 中的计算机视觉](https://docs.microsoft.com/learn/paths/explore-computer-vision-microsoft-azure/)、[探索自然语言处理](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/)以及[探索对话式 AI](https://docs.microsoft.com/learn/paths/explore-conversational-ai/)。这些练习配合教材提供，你可以使用其描述的方法进行练习。 

要完成这些练习，需要有一个 Microsoft Azure 订阅。如果讲师没有为你提供订阅，可以在 [https://azure.microsoft.com](https://azure.microsoft.com) 注册获取免费试用版。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 练习 |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
