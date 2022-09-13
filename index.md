---
title: 联机托管说明
permalink: index.html
layout: home
ms.openlocfilehash: 86fa2da9bfa9e4d7edb0c853f77e95fe69e97411
ms.sourcegitcommit: 3fc8440b350b498c2f50ab4ce531a0f906792584
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2022
ms.locfileid: "147866002"
---
# <a name="azure-ai-fundamentals-exercises"></a>Azure AI 基础知识练习

这些动手练习旨在支持 [Microsoft Learn](https://docs.microsoft.com/training/) 上的培训内容。

要完成这些练习，需要有一个 Microsoft Azure 订阅。 可在 [https://azure.microsoft.com](https://azure.microsoft.com) 注册免费试用版。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 练习 |
| ------- | 
{% 表示实验室 % 中的活动}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
