---
lab:
  title: 浏览 Azure AI 服务
---

# 浏览 Azure AI 服务

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

Azure AI 服务封装了常见的 AI 功能，这些功能可分为四个主要支柱：视觉、语音、语言和决策服务。 在本练习中，你将了解其中一个决策服务，大致了解如何在软件应用程序中预配和使用 Azure AI 服务资源。

你将在本练习中探索的具体 Azure AI 服务是异常检测器。 异常检测器用于分析一段时间内的数据值，并检测任何可能指示问题的异常值，以供进一步调查。 例如，温度控制存储设施中的传感器可能每分钟监视一次温度并记录测量值。 可使用异常检测器服务来分析记录的温度值，并标记任何明显超出预期温度正常范围的值。

为了测试异常情况检测服务的功能，我们将使用在 Cloud Shell 中运行的简单命令行应用程序。 相同的原则和功能适用于实际解决方案，如网站或手机应用。

> 注意：本练习的目标是大致了解 Azure AI 服务的预配和使用方式。 异常检测器用作示例，但在本练习中，你不需要全面了解异常情况检测！

## 创建异常检测器资源

首先，在 Azure 订阅中创建异常检测器资源：

1. 在另一个浏览器选项卡中，打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，搜索“异常检测器”，然后使用以下设置创建“异常检测器”资源：
    - **订阅**：Azure 订阅。
    - **资源组**：选择现有资源组或创建新资源组。
    - **区域**：选择任何可用区域。
    - **名称**：输入唯一名称。
    - **定价层**：免费 F0

1. 查看并创建资源。 等待部署完成，然后转到部署的资源。

1. 查看异常检测器资源的“密钥和终结点”页。 需要终结点和密钥才能从客户端应用程序进行连接。

## 运行 Cloud Shell

为了测试异常检测器服务的功能，我们将使用在 Azure 上的 Cloud Shell 中运行的简单命令行应用程序。

1. 在 Azure 门户中，选择搜索框右侧页面顶部的 [>_] (Cloud Shell) 按钮。 这会打开门户底部的 Cloud Shell 窗格。

    ![单击顶部搜索框右侧的图标启动 Cloud Shell](media/anomaly-detector/powershell-portal-guide-1.png)

1. 首次打开 Cloud Shell 时，系统可能会提示你选择要使用的 shell 类型（Bash 或 PowerShell）。 从列表中选择“PowerShell”。 如果看不到此选项，请跳过该步骤。  

1. 如果系统提示你为 Cloud Shell 创建存储，请确保已指定订阅，然后选择“创建存储”。 等待存储创建完毕，此过程大约需要一分钟。

    ![单击“确认”以创建存储。](media/anomaly-detector/powershell-portal-guide-2.png)

1. 确保 Cloud Shell 窗格左上角指示的 shell 类型已切换到 PowerShell。 如果是 Bash，请通过使用下拉菜单切换到 PowerShell。

    ![如何查找左侧下拉菜单以切换到 PowerShell](media/anomaly-detector/powershell-portal-guide-3.png)

1. 等待 PowerShell 启动。 你应在 Azure 门户中看到以下屏幕：  

    ![等待 PowerShell 启动。](media/anomaly-detector/powershell-prompt.png)

## 配置和运行客户端应用程序

现在，你已经有了 Cloud Shell 环境，可以运行简单应用程序，该应用程序使用异常检测器服务来分析数据。

1. 在命令行界面中，输入以下命令以下载示例应用程序并将其保存到名为 ai-900 的文件夹中。

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >提示：如果已在其他实验室中使用此命令克隆 ai-900 存储库，则可跳过此步骤。

1. 文件将下载到名为“ai-900”的文件夹中。 现在，我们想要查看 Cloud Shell 存储中的所有文件，并使用这些文件。 在 shell 中键入以下命令：

     ```PowerShell
    code .
    ```

    请注意此操作如何打开一个编辑器，如下图所示： 

    ![代码编辑器。](media/anomaly-detector/powershell-portal-guide-4.png)

1. 在左侧的“文件”窗格中，展开“ai-900”并选择“detect-anomalies.ps1”。 此文件包含一些使用异常情况检测服务的代码，如下所示：

    ![包含用于检测异常的代码的编辑器](media/anomaly-detector/detect-anomalies-code.png)

1. 不要太担心代码的细节，重要的是它需要终结点 URL 和异常检测器资源的任何一个密钥。 从资源的“密钥和终结点”页（仍应位于浏览器的顶部区域）复制这些信息，并将它们粘贴到代码编辑器，分别替换 YOUR_KEY 和 YOUR_ENDPOINT 占位符值。

    > 提示：使用“密钥和终结点”和“编辑器”窗格时，可能需要使用分隔条来调整屏幕区域  。

    粘贴密钥和终结点值后，代码的前两行应如下所示：

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. 在编辑器窗格的右上方，使用“...”按钮打开菜单，然后选择“保存”以保存更改。 然后再次打开菜单，并选择“关闭编辑器”。

    请记住，异常情况检测是一种人工智能技术，用于确定序列中的值是否在预期参数范围内。 示例客户端应用程序将使用异常检测器服务来分析包含一系列日期/时间及数值的文件。 应用程序应返回在每个时间点指示数值是否在预期参数内的结果。

1. 在 PowerShell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    cd ai-900
    .\detect-anomalies.ps1
    ```

1. 查看结果，请注意结果中的最后一列为 True 还是 False，以表明在每个日期/时间记录的值是否被视为异常。 考虑如何在真实情况下使用此信息。 如果这些值是冰箱温度或血压，并且检测到异常，应用程序会触发什么操作？  

## 清理

最好在项目结束时确定是否仍然需要所创建的资源。 持续运行资源可能会产生费用。 

如果继续学习其他 AI 基础知识模块，可保留资源以用于其他实验室。

如果已完成学习，则可以从 Azure 订阅中删除资源组或单个资源：

1. 在 [Azure 门户](https://portal.azure.com/)的“资源组”页面中，打开在创建资源时指定的资源组。

2. 单击“删除资源组”，键入资源组名称以确认要删除资源组，然后选择“删除”。 你还可以选择删除单个资源，操作方法：选择资源，单击三个点以显示更多选项，然后单击“删除”。