---
title: 联机托管说明
permalink: index.html
layout: home
ms.openlocfilehash: b85af520a10e63a2f9a5696db03bfd946aff968f
ms.sourcegitcommit: 1ef64e3008a439d0d0bb3d93a27d3df68d3d64a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2022
ms.locfileid: "140688688"
---
# <a name="azure-ai-fundamentals-exercises"></a>Azure AI 基础知识练习

使用以下链接完成针对 Microsoft 课程 [AI-900 Microsoft Azure AI 基础知识](https://docs.microsoft.com/learn/certifications/courses/ai-900t00)的动手实验室练习。

要完成这些练习，需要有一个 Microsoft Azure 订阅。 如果讲师未提供，可以在 [https://azure.microsoft.com](https://azure.microsoft.com) 注册免费试用版。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 练习 |
| ------- | 
{% 表示实验室 % 中的活动}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
