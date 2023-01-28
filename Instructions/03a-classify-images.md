---
lab:
  title: 探索图像分类
---

# <a name="explore-image-classification"></a>探索图像分类

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

计算机视觉认知服务提供有助于处理图像的预生成模型，但你将经常训练自己的计算机视觉模型。 例如，假设 Northwind Traders 零售公司希望创建自动结帐系统，该系统根据在结帐时摄像头拍摄的图像来识别客户想要购买的杂货物品。 为此，你将需要训练分类模型，该模型可对图像进行分类以识别要购买的物品。

在 Azure 中，可以使用自定义视觉认知服务来基于现有图像训练图像分类模型。 创建图像分类解决方案有两个元素。 首先，必须使用现有图像训练模型来识别不同的类。 然后，在训练模型后，必须将其发布为可由应用程序使用的服务。

为了测试自定义视觉服务的功能，我们将使用在 Cloud Shell 中运行的简单命令行应用程序。 相同的原则和功能适用于实际解决方案，如网站或手机应用。

## <a name="create-a-cognitive-services-resource"></a>创建“认知服务”资源

可通过创建“自定义视觉”资源或“认知服务”资源来使用自定义视觉服务。

>注意：并非所有资源在每个区域中均可用。 无论是创建自定义视觉资源还是创建认知服务资源，只有在[某些区域](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)创建的资源才能用于访问自定义视觉服务。 为简单起见，在以下配置说明中已为你预先选择了一个区域。

在 Azure 订阅中创建“认知服务”资源。

1. 在另一个浏览器选项卡中，打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，搜索“认知服务”，然后通过以下设置创建“认知服务”资源：
    - **订阅**：Azure 订阅。
    - **资源组**：选择或创建具有唯一名称的资源组。
    - **区域**：美国东部
    - **名称**：输入唯一名称。
    - 定价层：标准版 S0
    - **选中此框即表示我确认我已阅读并理解以下所有条款**：已选中。

1. 查看并创建资源，然后等待部署完成。 然后，转到部署的资源。

1. 查看认知服务资源的“密钥和终结点”页。 需要终结点和密钥才能从客户端应用程序进行连接。

## <a name="create-a-custom-vision-project"></a>创建自定义视觉项目

若要训练对象检测模型，需要基于训练资源创建自定义视觉项目。 为此，你将使用自定义视觉门户。

1. 从 https://aka.ms/fruit-images 下载并提取训练图像。 压缩文件夹中提供了这些图像，提取时，其中包含名为“苹果”、“香蕉”和“橘子”的子文件夹。

1. 在另一个浏览器选项卡中，打开自定义视觉门户 ([https://customvision.ai](https://customvision.ai?azure-portal=true))。 如果出现提示，请使用与你的 Azure 订阅关联的 Microsoft 帐户进行登录，并同意服务条款。

1. 在自定义视觉门户中，创建一个具有以下设置的新项目：

    - 名称：杂货结帐
    - 说明：杂货的图像分类
    - 资源：先前创建的自定义视觉资源
    - **项目类型**：分类
    - 分类类型：多类（每个图像一个标记）
    - 域：食品

1. 单击“添加图像”，并选择你之前提取的“苹果”文件夹中的所有文件 。 然后上传图像文件，指定标记“苹果”，如下所示：

    ![上传带有“苹果”标记的苹果](media/create-image-classification-system/upload-apples.jpg)

1. 重复上一步操作，上传“香蕉”文件夹中带有标记“香蕉”的图像，以及“橘子”文件夹中带有标记“橘子”的图像。

1. 浏览已在自定义视觉项目中上传的图像 - 每个类应有 15 个图像，如下所示：

    ![已标记的水果图像 - 15 个苹果、15 个香蕉和 15 个橘子](media/create-image-classification-system/fruit.jpg)

1. 在自定义视觉项目的图像上方，单击“训练”以使用已标记的图像训练分类模型。 选择“快速训练”选项，然后等待训练迭代完成（这可能需要一分钟左右）。

1. 模型迭代已训练后，请查看“精度”、“召回率”和“AP”性能指标 - 这些指标度量分类模型的预测准确性，并且应该都很高。

## <a name="test-the-model"></a>测试模型

在发布此模型迭代供应用程序使用之前，应对其进行测试。

1. 在性能指标上方，单击“快速测试”。

1. 在“图像 URL”框中，键入 `https://aka.ms/apple-image` 并单击“&#10132;”

1. 查看模型返回的预测 -“苹果”的概率分数应为最高，如下所示：

    ![带有“苹果”的类预测的图像](media/create-image-classification-system/test-apple.jpg)

1. 关闭“快速测试”窗口。

## <a name="publish-the-image-classification-model"></a>发布图像分类模型

现在，你已准备好发布已训练模型并从客户端应用程序使用它。

1. 单击“&#128504; 发布”以通过以下设置发布已训练模型：
    - 模型名称：杂货
    - 预测资源：先前创建的预测资源。

1. 发布后，单击“预测 URL”(&#127760;) 图标，查看使用已发布模型所需的信息。 稍后，你将需要相应的 URL 和 Prediction-Key 值从图像 URL 中获取预测，因此请保持此对话框处于打开状态，然后继续执行下一个任务。 

## <a name="run-cloud-shell"></a>运行 Cloud Shell

为了测试自定义视觉服务的功能，我们将使用在 Azure 上的 Cloud Shell 中运行的简单命令行应用程序。

1. 在 Azure 门户中，选择搜索框右侧页面顶部的 [>_] (Cloud Shell) 按钮。 这会打开门户底部的 Cloud Shell 窗格。 

    ![单击顶部搜索框右侧的图标启动 Cloud Shell](media/create-image-classification-system/powershell-portal-guide-1.png)

1. 首次打开 Cloud Shell 时，系统可能会提示你选择要使用的 shell 类型（Bash 或 PowerShell）。 从列表中选择“PowerShell”。 如果看不到此选项，请跳过该步骤。  

1. 如果系统提示你为 Cloud Shell 创建存储，请确保已指定订阅，然后选择“创建存储”。 等待存储创建完毕，此过程大约需要一分钟。

    [![单击“确认”以创建存储。](media/create-image-classification-system/powershell-portal-guide-2.png)](media/create-image-classification-system/powershell-portal-guide-2.png#lightbox)

1. 请确保 Cloud Shell 窗格左上角指示的 shell 类型切换到 PowerShell。 如果是 Bash，请通过使用下拉菜单切换到 PowerShell。

    ![如何查找左侧下拉菜单以切换到 PowerShell](media/create-image-classification-system/powershell-portal-guide-3.png)

1. 等待 PowerShell 启动。 你应在 Azure 门户中看到以下屏幕：  

    ![等待 PowerShell 启动。](media/create-image-classification-system/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>配置和运行客户端应用程序

现在，你已经有了 Cloud Shell 环境，可以运行简单应用程序，使用自定义视觉服务来分析图像。

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

    ![代码编辑器。](media/create-image-classification-system/powershell-portal-guide-4.png)

1. 在左侧的“文件”窗格中，展开“ai-900”并选择“classify-image.ps1”。 此文件包含使用自定义视觉模型分析图像的一些代码，如下所示：

     ![包含对图像进行分类的代码的编辑器](media/create-image-classification-system/classify-image-code.png)

1. 不要太担心代码的细节，重要的是在使用图像 URL 时需要预测 URL 和自定义视觉模型的密钥。 

   从自定义视觉项目的对话框中获取预测 URL。 

   >注意：请记住，在发布图像分类模型后，你查看了预测 URL。 若要查找预测 URL，请导航至项目中的“性能”选项卡，然后单击“预测 URL”（如果屏幕已压缩，则可能只会看到一个地球图标）。 随即将显示一个对话框。 复制“如果有图像 URL”的 URL。 将其粘贴到代码编辑器中，从而替换 YOUR_PREDICTION_URL。

    使用同一个对话框，获取预测密钥。 复制 Set Prediction-Key Header to 之后显示的预测密钥。 将其粘贴到代码编辑器中，从而替换 YOUR_PREDICTION_KEY 占位符值。

    ![预测 URL 的屏幕截图。](media/create-image-classification-system/find-prediction-url.png)

    粘贴“预测 URL”和“预测密钥”值后，前两行代码应如下所示：

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

1. 在编辑器窗格的右上方，使用“...”按钮打开菜单，然后选择“保存”以保存更改。 然后再次打开菜单，并选择“关闭编辑器”。

    你将使用示例客户端应用程序将多个图像分类为“苹果”、“香蕉”或“橘子”类别。

1. 我们将对此图像进行分类：

    ![苹果的图像](media/create-image-classification-system/fruit-1.jpg)

    在 PowerShell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    cd ai-900
    ./classify-image.ps1 1
    ```

1. 查看预测，该预测应为“苹果”。

1. 现在，让我们尝试另一张图像：

    ![香蕉的图像](media/create-image-classification-system/fruit-2.jpg)

    运行以下命令：

    ```PowerShell
    ./classify-image.ps1 2
    ```

1. 验证模型是否将此图像分类为“香蕉”。

1. 最后，尝试第三个测试图像：

    ![橘子的图像](media/create-image-classification-system/fruit-3.jpg)

    运行以下命令：

    ```PowerShell
    ./classify-image.ps1 3
    ```

1. 验证模型是否将此图像分类为“橘子”。

## <a name="learn-more"></a>了解详细信息

这个简单的应用仅显示自定义视觉服务的部分功能。 若要详细了解此服务的更多用途，请参阅[“自定义视觉”页](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)。
