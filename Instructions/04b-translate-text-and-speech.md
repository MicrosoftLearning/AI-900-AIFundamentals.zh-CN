---
lab:
  title: 探索翻译
---

# 探索翻译

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

推动人类文明发展的驱动力之一是相互交流的能力。 在大多数人类工作中，通信是关键。

人工智能 (AI) 可以通过在语言之间翻译文本或语音来帮助简化通信，帮助消除不同国家/地区和文化之间的沟通障碍。

为了测试翻译服务的功能，我们将使用在 Cloud Shell 中运行的简单命令行应用程序。 相同的原则和功能适用于实际解决方案，如网站或手机应用。

## 创建 Azure AI 服务资源

可以通过创建翻译器**** 资源或 Azure AI 服务**** 资源来使用翻译器服务。

在 Azure 订阅中创建一个 Azure AI 服务资源（如果尚未这样做）。

1. 在另一个浏览器选项卡中，打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，然后搜索“Azure AI 服务”。 选择创建 Azure AI 服务计划。 随后你会转到一个页面，可在其中创建 Azure AI 服务资源。 使用以下设置对其进行配置：
    - **订阅**：Azure 订阅。
    - **资源组**：选择或创建具有唯一名称的资源组。
    - **区域**：选择任何可用区域。
    - **名称**：输入唯一名称。
    - 定价层：标准版 S0
    - **选中此框即表示我确认我已阅读并理解以下所有条款**：已选中。

1. 查看并创建资源，然后等待部署完成。 然后，转到部署的资源。

1. 查看 Azure AI 服务资源的“密钥和终结点”页。 需要密钥和位置，以从客户端应用程序进行连接。

### 获取 Azure AI 服务资源的密钥和位置

1. 等待部署完成。 然后转到 Azure AI 服务资源，在“概述”**** 页上选择用于管理服务密钥的链接。 需要密钥和位置才能从客户端应用程序连接到 Azure AI 服务资源。

1. 查看资源的“密钥和终结点”页。 需要位置/区域和密钥才能从客户端应用程序进行连接。

> **备注**：要使用翻译器服务，无需使用 Azure AI 服务终结点。 仅为翻译器服务提供全局终结点。 

## 运行 Cloud Shell

为了测试翻译服务的功能，我们将使用在 Azure 上的 Cloud Shell 中运行的简单命令行应用程序。 

1. 在 Azure 门户中，选择搜索框右侧页面顶部的 [>_] (Cloud Shell) 按钮。 这会打开门户底部的 Cloud Shell 窗格。

    ![单击顶部搜索框右侧的图标启动 Cloud Shell](media/translate-text-and-speech/powershell-portal-guide-1.png)

1. 首次打开 Cloud Shell 时，系统可能会提示你选择要使用的 shell 类型（Bash 或 PowerShell）。 从列表中选择“PowerShell”。 如果看不到此选项，请跳过该步骤。  

1. 如果系统提示你为 Cloud Shell 创建存储，请确保已指定订阅，然后选择“创建存储”。 等待存储创建完毕，此过程大约需要一分钟。

    ![单击“确认”以创建存储。](media/translate-text-and-speech/powershell-portal-guide-2.png)

1. 确保 Cloud Shell 窗格左上角指示的 shell 类型已切换到 PowerShell。 如果是 Bash，请通过使用下拉菜单切换到 PowerShell。 

    ![如何查找左侧下拉菜单以切换到 PowerShell](media/translate-text-and-speech/powershell-portal-guide-3.png) 

1. 等待 PowerShell 启动。 你应在 Azure 门户中看到以下屏幕：  

    ![等待 PowerShell 启动。](media/translate-text-and-speech/powershell-prompt.png)

## 配置和运行客户端应用程序

现在，你已有一个自定义模型，可以运行使用翻译服务的简单客户端应用程序。

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

    ![代码编辑器。](media/translate-text-and-speech/powershell-portal-guide-4.png)

1. 在左侧的“文件”窗格中，展开“ai-900”并选择“translator.ps1”。 此文件包含一些使用翻译器服务的代码：

    ![包含使用翻译器服务的代码的编辑器](media/translate-text-and-speech/translate-code.png)

1. 不要太担心代码的细节，重要的是它需要区域/位置和 Azure AI 服务资源的任一密钥。 从 Azure 门户中的资源的“密钥和终结点”页复制这些信息，并将它们粘贴到代码编辑器，分别替换 YOUR_KEY 和 YOUR_LOCATION 占位符值。

    粘贴密钥和位置值后，代码的第一行应如下所示：

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $location="somelocation"
    ```

1. 在编辑器窗格的右上方，使用“...”按钮打开菜单，然后选择“保存”以保存更改。 然后再次打开菜单，并选择“关闭编辑器”。

    示例客户端应用程序将使用翻译器服务执行多个任务：
    - 将文本从英语翻译为法语、意大利语和中文。
    - 将音频从英语翻译为法语文本

    使用以下视频播放器收听应用程序将处理的输入音频：

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWORN0" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>


    > 注意：实际的应用程序可以接受麦克风的输入并将响应发送给扬声器，但在这个简单的示例中，我们将在音频文件中使用预先录制的输入。

1. 在 Cloud Shell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    cd ai-900
    ./translator.ps1
    ```

1. 查看输出。 你是否看到了从英语文本到法语、意大利语和中文的翻译？  你是否看到了英语音频“hello”翻译为法语文本？

## 了解更多

这个简单的应用只显示了翻译器服务的某些功能。 要详细了解此服务的更多用途，请参阅[“翻译器”页](https://docs.microsoft.com/azure/cognitive-services/translator/translator-overview)。