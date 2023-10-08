---
lab:
  title: 探索表单识别
---

# 探索表单识别

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

在计算机视觉的人工智能 (AI) 领域中，通常使用光学字符识别 (OCR) 来读取打印或手写的文档。 通常，只需将文本从文档中提取为一种可用于进一步处理或分析的格式。

更高级的 OCR 方案是从表单（如采购订单或发票）中提取信息，并从语义上了解表单中的字段代表什么。 表单识别器服务是专门为此类 AI 问题设计的。

表单识别器使用经过训练的机器学习模型从发票、回执等图像中提取文本。 尽管其他计算机视觉模型可以捕获文本，但表单识别器还会捕获文本的结构，如表中的键/值对和信息。 这样，就可以自动捕获原始文件中的文本之间的关系，而不是将条目从表单手动输入到数据库中。 

为了测试表单识别器服务的功能，我们将使用在 Cloud Shell 中运行的简单命令行应用程序。 相同的原则和功能适用于实际解决方案，如网站或手机应用。

## 创建 Azure AI 服务资源

可通过创建“表单识别器”资源或“Azure AI 服务”资源来使用表单识别器服务 。

在 Azure 订阅中创建一个 Azure AI 服务资源（如果尚未这样做）。

1. 在另一个浏览器选项卡中，打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，然后搜索“Azure AI 服务”。 选择创建一个 Azure AI 服务计划。 随后你会转到一个页面，可在其中创建 Azure AI 服务资源。 使用以下设置对其进行配置：
    - **订阅**：Azure 订阅。
    - **资源组**：选择或创建具有唯一名称的资源组。
    - **区域**：选择任何可用区域。
    - **名称**：输入唯一名称。
    - 定价层：标准版 S0
    - **选中此框即表示我确认我已阅读并理解以下所有条款**：已选中。

1. 查看并创建资源，然后等待部署完成。 然后，转到部署的资源。

1. 查看 Azure AI 服务资源的“密钥和终结点”页。 需要终结点和密钥才能从客户端应用程序进行连接。

## 运行 Cloud Shell

为了测试表单识别器服务的功能，我们将使用在 Azure 上的 Cloud Shell 中运行的简单命令行应用程序。 

1. 在 Azure 门户中，选择搜索框右侧页面顶部的 [>_] (Cloud Shell) 按钮。 这会打开门户底部的 Cloud Shell 窗格。 

    ![单击顶部搜索框右侧的图标启动 Cloud Shell](media/analyze-receipts/powershell-portal-guide-1.png)

1. 首次打开 Cloud Shell 时，系统可能会提示你选择要使用的 shell 类型（Bash 或 PowerShell）。 从列表中选择“PowerShell”。 如果看不到此选项，请跳过该步骤。  

1. 如果系统提示你为 Cloud Shell 创建存储，请确保已指定订阅，然后选择“创建存储”。 等待存储创建完毕，此过程大约需要一分钟。

    ![单击“确认”以创建存储。](media/analyze-receipts/powershell-portal-guide-2.png)

1. 请确保 Cloud Shell 窗格左上角指示的 shell 类型切换到 PowerShell。 如果是 Bash，请通过使用下拉菜单切换到 PowerShell。

    ![如何查找左侧下拉菜单以切换到 PowerShell](media/analyze-receipts/powershell-portal-guide-3.png) 

1. 等待 PowerShell 启动。 你应在 Azure 门户中看到以下屏幕：  

    ![等待 PowerShell 启动。](media/analyze-receipts/powershell-prompt.png) 

## 配置和运行客户端应用程序

现在，你已有一个自定义模型，可以运行使用表单识别器服务的简单客户端应用程序。

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

    ![代码编辑器。](media/analyze-receipts/powershell-portal-guide-4.png)

1. 在左侧的“文件”窗格中，展开“ai-900”并选择“form-recognizer.ps1”。 此文件包含一些使用表单识别器服务分析回执中字段的代码，如下所示：

    ![该编辑器包含用于分析回执中字段的代码。](media/analyze-receipts/recognize-receipt-code.png)

1. 不要太担心代码的细节，重要的是它需要终结点 URL 和 Azure AI 服务资源的其中一个密钥。 从 Azure 门户中的资源的“密钥和终结点”页复制这些信息，并将它们粘贴到代码编辑器，分别替换 YOUR_KEY 和 YOUR_ENDPOINT 占位符值。

    > 提示：使用“密钥和终结点”和“编辑器”窗格时，可能需要使用分隔条来调整屏幕区域  。

    粘贴密钥和终结点值后，代码的前两行应如下所示：

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. 在编辑器窗格的右上方，使用“...”按钮打开菜单，然后选择“保存”以保存更改。 然后再次打开菜单，并选择“关闭编辑器”。 现在，你已设置了密钥和终结点，可以使用资源来分析回执中的字段。 在这种情况下，需要使用表单识别器的内置模型来分析虚构的 Northwind Traders 零售公司的回执。

    示例客户端应用程序将分析下图：

    ![这是一张回执的图像。](media/analyze-receipts/receipt.jpg)

1. 在 PowerShell 窗格中，输入以下命令运行代码以读取文本：

    ```PowerShell
    cd ai-900
    ./form-recognizer.ps1
    ```

1. 查看返回的结果。 从结果可了解到，表单识别器可以解释表单中的数据，正确识别商家地址和电话号码，以及交易日期和时间，还有明细项目、小计、税款和总金额。

## 了解更多

这个简单的应用仅显示计算机视觉服务的部分表单识别器功能。 要详细了解此服务的更多用途，请参阅[“表单识别器”页](https://docs.microsoft.com/azure/applied-ai-services/form-recognizer/overview)。
