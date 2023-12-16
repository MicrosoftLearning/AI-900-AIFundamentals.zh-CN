---
lab:
  title: 探索文本分析
---

# 探索文本分析

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

自然语言处理 (NLP) 是人工智能 (AI) 的一个分支，处理书面语言和口语。 可以使用 NLP 生成解决方案，从文本或语音中提取语义含义，或者以自然语言构建有意义的响应。

Microsoft Azure AI 服务** 包含语言** 服务中的文本分析功能，它提供一些开箱即用的 NLP 功能，包括识别文本中的关键短语，以及基于情绪对文本进行分类。

例如，假设虚构的 Margie's Travel 组织鼓励客户提交酒店住宿评论。 可使用语言服务通过提取关键短语来汇总评论，确定哪些评论是正面的，哪些是负面的，或者分析评论文本提到的已知实体（如位置或人员）。

为了测试语言服务的功能，我们将使用在 Cloud Shell 中运行的简单命令行应用程序。 相同的原则和功能适用于实际解决方案，如网站或手机应用。

## 创建 Azure AI 服务资源

可以通过创建语言**** 资源或 Azure AI 服务**** 资源来使用语言服务。

在 Azure 订阅中创建一个 Azure AI 服务资源（如果尚未这样做）。

1. 在另一个浏览器选项卡中，打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，然后搜索“Azure AI 服务”。 选择创建 Azure AI 服务计划。 随后你会转到一个页面，可在其中创建 Azure AI 服务资源。 使用以下设置对其进行配置：
    - **订阅**：Azure 订阅。
    - **资源组**：选择或创建具有唯一名称的资源组。
    - **区域**：选择任何可用区域。
    - **名称**：输入唯一名称。
    - 定价层：标准版 S0
    - **选中此框即表示我确认我已阅读并理解以下所有条款**：已选中。

1. 查看并创建资源。

### 获取 Azure AI 服务资源的密钥和终结点

1. 等待部署完成。 然后转到 Azure AI 服务资源，在“概述”**** 页上选择用于管理服务密钥的链接。 需要终结点和密钥才能从客户端应用程序连接到 Azure AI 服务资源。

1. 查看资源的“密钥和终结点”页。 需要有密钥和终结点才能从客户端应用程序进行连接。

## 运行 Cloud Shell

为了测试语言服务的文本分析功能，我们将使用在 Azure 上的 Cloud Shell 中运行的简单命令行应用程序。

1. 在 Azure 门户中，选择搜索框右侧页面顶部的 [>_] (Cloud Shell) 按钮。 这会打开门户底部的 Cloud Shell 窗格。

    ![单击顶部搜索框右侧的图标启动 Cloud Shell](media/analyze-text-language-service/powershell-portal-guide-1.png)

1. 首次打开 Cloud Shell 时，系统可能会提示你选择要使用的 shell 类型（Bash 或 PowerShell）。 从列表中选择“PowerShell”。 如果看不到此选项，请跳过该步骤。  

1. 如果系统提示你为 Cloud Shell 创建存储，请确保已指定订阅，然后选择“创建存储”。 等待存储创建完毕，此过程大约需要一分钟。

    ![单击“确认”以创建存储。](media/analyze-text-language-service/powershell-portal-guide-2.png)

1. 确保 Cloud Shell 窗格左上角指示的 shell 类型已切换到 PowerShell。 如果是 Bash，请通过使用下拉菜单切换到 PowerShell。

    ![如何查找左侧下拉菜单以切换到 PowerShell](media/analyze-text-language-service/powershell-portal-guide-3.png)

1. 等待 PowerShell 启动。 你应在 Azure 门户中看到以下屏幕：  

    ![等待 PowerShell 启动。](media/analyze-text-language-service/powershell-prompt.png)

## 配置和运行客户端应用程序

现在，你已有一个自定义模型，可以运行使用语言服务的简单客户端应用程序。

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

    ![代码编辑器。](media/analyze-text-language-service/powershell-portal-guide-4.png)

1. 在左侧的“文件”窗格中，展开“ai-900”并选择“analyze-text.ps1”。 此文件包含一些使用语言服务的代码：

    ![包含使用语言服务的代码的编辑器](media/analyze-text-language-service/analyze-text-code.png)

1. 不要太担心代码的详细信息。 在 Azure 门户中，导航到 Azure AI 服务资源。 然后选择左侧窗格中的“密钥和终结点”页。 从该页中复制密钥和终结点，并将它们粘贴到代码编辑器，分别替换 YOUR_KEY 和 YOUR_ENDPOINT 占位符值。

    > 提示：使用“密钥和终结点”和“编辑器”窗格时，可能需要使用分隔条来调整屏幕区域  。

    ![在 Azure AI 服务资源的左侧窗格中查找“密钥和终结点”选项卡。](media/analyze-text-language-service/key-endpoint-support.png)

    替换密钥和终结点值后，代码的第一行应如下所示：

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    ```

1. 在编辑器窗格的右上方，使用“...”按钮打开菜单，然后选择“保存”以保存更改。 然后再次打开菜单，并选择“关闭编辑器”。

    示例客户端应用程序将使用 Azure AI 服务的语言服务来检测语言、提取关键短语、确定情绪以及提取评论的已知实体。

1. 在 Cloud Shell 中，输入以下命令以运行代码：

    ```PowerShell
    cd ai-900
    ./analyze-text.ps1 review1.txt
    ```

    你将看到以下文本：

    >优质酒店和工作人员 英国伦敦皇家酒店 2018/3/2 干净的客房，良好的服务，靠近白金汉宫和威斯敏斯特教堂的绝佳位置，等等。 我们住得很愉快。 庭院非常宁静。我们去了同一酒店的一个印度风的米其林星餐厅（西海岸的鱼很多）， 菜非常美味。 房间布置得很好，有厨房、休息室、卧室和巨大的浴室。 极力推荐。

1. 查看输出。

1. 在 PowerShell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    ./analyze-text.ps1 review2.txt
    ```

    你将看到以下文本：

    >服务差的怠慢酒店 英国伦敦皇家酒店 2018/5/6 这是一家老酒店（大约从 20 世纪 50 年代就有了），房间陈设一般 - 现在有点旧了，需要改变。 无法正常上网，我不得不去他们的办公室办理回家的登机手续。 网站说它靠近英国博物馆，但步行要走很远。

1. 查看输出。

1. 在 PowerShell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    ./analyze-text.ps1 review3.txt
    ```

    你将看到以下文本：

    >位置很好，员工也很能提供帮助，但却坐落于繁忙街道上。
    美国旧金山 Lombard 酒店 2018/8/16 我们看了评论后便于八月住在这里。 我们对这里的位置非常满意，就在栗树街后面，这是一个国际化和时尚的区域，有很多餐馆可供选择。 Marina 地区很适合闲逛，那里有非常有趣的房子。 一定要步行到旧金山艺术博物馆和 Marina，去感受金门大桥和城市的好风光。 顺着公交线路很容易到达市中心。 房间非常整洁，且拥有很多小房间，员工友好且乐于助人。 唯一不好的地方是 Lombard 街的噪音，所以要住一个远离交通噪音的房间。

1. 查看输出。

1. 在 PowerShell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    ./analyze-text.ps1 review4.txt
    ```

    你将看到以下文本：

    >非常吵闹，房间很小 美国旧金山 Lombard 酒店 2018/9/5 酒店位于 Lombard 街，这是一个非常繁忙的六车道街道，直通金门大桥。 从清晨到深夜一直车来车往，尤其是周末。 如果房间有更好的隔音，噪声就不会那么吵，但事实并非如此。 我得把棉球塞进耳朵里才能睡着，导致我第二天累到无法好好游玩城市。 房间非常小。 我选择这个房间是因为里面有两张双人床，但房间里几乎没有空间放这两张床。 四个家庭成员在这个房间里显得很挤。 尽管如此，房间还是很干净的，他们也在努力对房间进行更新。 酒店位于 Marina 地区，那里有很多好吃的地方，步行就能到达 Presidio。 对于有预算安排的年轻熬夜成年人来说，这可能是个不错的酒店

1. 查看输出。

## 了解更多

这个简单的应用只显示了语言服务的部分功能。 要详细了解此服务的用途，请参阅[语言服务页面](https://azure.microsoft.com/services/cognitive-services/language-service/)。
