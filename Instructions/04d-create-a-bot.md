---
lab:
  title: 探索问题解答
---

# 探索问题解答

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

对于客户支持方案，通常的做法是创建一个机器人，该机器人可以通过网站聊天窗口、电子邮件或语音界面解释和回答常见问题。 机器人界面的基础是问题和适当答案的知识库，机器人可以在其中搜索合适的答案进行响应。

## 创建自定义问题解答知识库

语言服务的自定义问题解答功能让你可以通过输入问答对或从现有文档或网页中快速创建知识库。 然后，它可以使用一些内置的自然语言处理功能来解释问题并找到合适的答案。

1. 打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，搜索“语言服务”，并使用以下设置创建“语言服务”资源，然后单击“继续创建资源”：选择其他功能
    - **默认功能**：保留默认功能。
    - **自定义功能**：选择自定义问题解答。

    ![创建启用了自定义问题解答的语言服务资源。](media/create-a-bot/create-language-service-resource.png)

1. 在“创建语言”页上指定以下设置：
    - **订阅**：Azure 订阅。
    - **资源组**：选择现有资源组或创建新资源组。
    - **名称**：语言资源的唯一名称。
    - **定价层**：S（每分钟 1000 次调用）
    - **Azure 搜索区域**：任何可用位置。
    - **Azure 搜索定价层**：免费 F（3 个索引）-（如果此层不可用，请选择“标准 S”（50 个索引））
    - **选中此框即表示我已查看并确认负责任 AI 声明中的条款**：已选中。

    > **备注**：如果已预配免费层 Azure 认知搜索**** 资源，则配额可能不支持再创建另一个资源。 在这种情况下，请选择“免费 F”以外的其他层。

1. 单击“查看和创建”，然后单击“创建”。 等待部署支持自定义问题解答知识库的语言服务。

1. 在新的浏览器标签页中，打开“Language Studio”门户 ([https://language.azure.com](https://language.azure.com?azure-portal=true))，然后使用与 Azure 订阅关联的 Microsoft 帐户登录。

1. 如果系统提示选择语言资源，请选择以下设置：
    - **Azure 目录**：包含订阅的 Azure 目录。
    - **订阅**：Azure 订阅。
    - **语言资源**：先前创建的语言资源。

1. 如果系统未提示你选择语言资源，原因可能是订阅中有多个语言资源；在这种情况下，你应该：
    1. 在页面顶部的栏中，单击“设置(&#9881;)”按钮。
    2. 在“设置”页上，查看“资源”选项卡。
    3. 选择刚刚创建的语言资源，然后单击“切换资源”。
    4. 在页面顶部，单击“Language Studio”以返回到 Language Studio 主页。

1. 在 Language Studio 门户顶部的“新建”菜单中，选择“自定义问题解答”。

1. 在“选择资源‘你的资源’的语言设置”页上，选择“我想要在此资源中创建项目时选择语言”，然后单击“下一步”  。

1. 在“输入基本信息”页上，输入以下详细信息，然后单击“下一步”：
    - **语言资源**：选择语言资源。  
    - **Azure 搜索资源**：选择 Azure 搜索资源。
    - **名称**：MargiesTravel
    - **说明**：简单的知识库
    - **源语言**：英语
    - **未返回答案时的默认答案**：找不到答案

1. 在“检查并完成”页上，单击“创建项目”。

1. 你将访问“管理源”页。 单击“&#65291;添加源”，然后选择“URL”。

1. 在“添加 URL”框中，单击“+ 添加 URL” 。 键入以下内容，然后选择“全部添加”：
    - **URL 名称**：MargiesKB
    - **URL**：`https://raw.githubusercontent.com/MicrosoftLearning/AI-900-AIFundamentals/main/data/qna/margies_faq.docx`
    - **对文件结构进行分类**：自动检测 

## 编辑知识库

你的知识库基于常见问题解答文档中的详细信息和一些预定义的答复。 可以添加自定义问答对来补充这些问题。

1. 单击左侧面板上的“编辑知识库”。 然后单击“+ 添加问题对”。

1. 在“问题”框中，键入 `Hello`，然后单击“提交更改” 。

1. 单击“+ 添加备用短语”并键入 `Hi`，然后单击“提交更改” 。

1. 在“答案和提示”框中，键入 ****。 将“源”保持：编辑状态。

1. 单击“提交”  。 然后，在页面顶部单击“保存更改”。 可能需要更改窗口的大小才能看到该按钮。

## 训练和测试知识库

现在，你已拥有知识库，可以对其进行测试。

1. 在页面顶部，单击“测试”，以测试知识库。

1. 在测试窗格的底部，输入消息“Hi”。 应会返回答复“Hello”。

1. 在测试窗格的底部，输入消息“我想预订航班”。 应会返回来自常见问题解答的适当答复。

    > 注意：答复包括一个简短回答以及一个更详细的回答段落 - 回答段落显示常见问题解答文档中最接近匹配问题的全文，而简短回答是从段落中智能提取的内容 。 可使用测试窗格顶部的“显示简短回答”复选框来控制是否从答复中显示简短回答。

1. 请尝试其他问题，例如“如何取消预订?”

1. 完成知识库测试后，单击“测试”以关闭测试窗格。

## 为知识库创建机器人

该知识库提供了一个后端服务，客户端应用程序可以使用该服务通过某种用户界面回答问题。 通常情况下，这些客户端应用程序就是机器人。 若要使机器人可以使用知识库，必须将其发布为可通过 HTTP 访问的服务。 然后，可以使用 Azure 机器人服务创建并托管使用知识库的机器人来回答用户问题。

1. 在“Language Studio”页的左侧，单击“部署知识库”。

1. 在页面顶部，单击“部署”。 一个对话框会询问是否要部署项目。 选择“部署”。

1. 部署服务后，单击“创建机器人”。 这会在新的浏览器选项卡中打开 Azure 门户，以便可以在 Azure 订阅中创建 Web 应用机器人。

1. 在 Azure 门户中创建 Web 应用机器人。 （你可能会看到一条警告消息，提醒检查模板源是否可信。 无需对该消息执行任何操作。）通过更新以下设置继续：

    - **项目详细信息**
        - **订阅**：Azure 订阅
        - 资源组：包含语言资源的资源组
    - **实例详细信息**
        - 资源组位置：与你的语言服务相同的位置。
    - **Azure 机器人**
        - 机器人句柄：机器人的唯一名称（预填充）
    - **选择定价层**
        - 定价层：免费 (F0)（可能需要选择“更改计划”）
    - **Microsoft 应用 ID**
        - 创建类型：选择“创建新的用户分配的托管标识” 

5. 选择“下一步：Web 应用 >”以继续更新设置。 
    - **应用服务**
        - **应用名称**：与“机器人句柄”相同，但会自动追加“.azurewebsites.net”**
        - **SDK 语言**：选择 C# 或 Node.js
    - **应用服务计划**
        - 创建类型：选择“创建新的应用服务计划”
    - **应用设置**
        - 语言资源密钥：需要复制语言资源密钥并将其粘贴到此处。 
        
        > 注意：若要导航到语言资源密钥，请打开 [https://portal.azure.com](https://portal.azure.com?azure-portal=true)。 在主页上，单击“资源组”，找到在其中创建语言资源的资源组。 选择语言资源并导航到其左侧菜单。 然后选择“密钥和终结点”。 复制其中一个密钥。 

    -  
        - 语言项目名称：MargiesTravel
        - 语言服务终结点主机名：已使用你的语言服务终结点预先填充
    - **语言服务详细信息**
        - 订阅 ID：已使用你的订阅 ID 预先填充
        - 资源组名称：已使用你的资源组名称预先填充
        - 帐户名称：已使用你的资源名称预先填充

1. 选择“查看 + 创建”。

1. 等待机器人创建完毕（右上方看起来像铃铛的通知图标会在等待时动态显示）。 然后在部署已完成的通知中，单击“转到资源”（或者，在主页上，单击“资源组”，打开你在其中创建 Web 应用机器人的资源组并单击它。）

1. 在机器人的左侧窗格中查找“设置”，单击“在 Web 聊天中测试”，然后等待机器人显示消息“欢迎使用”（初始化过程可能需要几秒钟时间）  。

1. 使用测试聊天界面确保机器人按照预期回答知识库中的问题。 例如，尝试提交“我需要取消酒店预订”。

试用机器人。 你可能会发现它可以非常准确地回答常见问题解答中的问题，但它解答未经训练的问题的能力有限。 可以随时使用 Language Studio 编辑知识库以对其进行改进，然后重新发布它。

## 了解更多

- 若要了解问题解答服务的详细信息，请参阅[文档](https://docs.microsoft.com/azure/cognitive-services/language-service/question-answering/overview)。
- 若要详细了解 Microsoft 机器人服务，请查看 [Azure 机器人服务页](https://azure.microsoft.com/services/bot-service/)。
