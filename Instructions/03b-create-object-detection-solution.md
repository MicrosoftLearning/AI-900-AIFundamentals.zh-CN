---
lab:
  title: 探索对象检测
---

# 探索对象检测

> 注意：要完成此实验室，需要一个你在其中具有管理权限的 [Azure 订阅](https://azure.microsoft.com/free?azure-portal=true)。

对象检测是计算机视觉的一种形式，在其中训练机器学习模型以对图像中的各个对象实例进行分类并指示标记其位置的边界框。 可以将其视为从图像分类（模型回答“这属于哪种图像？”这一问题）到生成解决方案（我们可以询问模型“此图像中有哪些对象，以及它们在何处？”）的过程。

例如，道路安全计划可能会将行人和骑自行车的人确定为交叉口最脆弱的道路使用者。 通过使用摄像头监视十字路口，可以分析道路使用者的图像，以检测行人和骑自行车的人，以便监视他们的号码，甚至改变交通信号的行为。

Microsoft Azure 中的自定义视觉认知**** 服务提供基于云的解决方案，用于创建和发布自定义对象检测模型。 在 Azure 中，可以使用自定义视觉服务来基于现有图像训练物体检测模型。 创建物体检测解决方案有两个元素。 首先，必须使用标记图像训练模型以检测物体的位置和种类。 然后，在训练模型后，必须将其发布为可由应用程序使用的服务。

为了测试自定义视觉服务的功能以检测图像中的对象，我们将使用在 Cloud Shell 中运行的简单命令行应用程序。 这些原则和功能同样适用于实际的解决方案，如网站或移动应用。

## 创建 Azure AI 服务资源

可通过创建“自定义视觉”资源或“Azure AI 服务”资源来使用自定义视觉服务。

> 注意：并非所有资源在每个区域中均可用。 无论是创建自定义视觉资源还是 Azure AI 服务资源，都只能使用在某些区域[](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)中创建的资源来访问自定义视觉服务。 为简单起见，在以下配置说明中已为你预先选择了一个区域。

在 Azure 订阅中创建“Azure AI 服务”资源。

1. 在另一个浏览器选项卡中，打开 Azure 门户 ([https://portal.azure.com](https://portal.azure.com?azure-portal=true))，并登录 Microsoft 帐户。

1. 单击“&#65291;创建资源”按钮，然后搜索“Azure AI 服务”。 选择创建 Azure AI 服务计划。 随后你会转到一个页面，可在其中创建 Azure AI 服务资源。 使用以下设置对其进行配置：
    - **订阅**：Azure 订阅。
    - **资源组**：选择或创建具有唯一名称的资源组。
    - **区域**：美国东部
    - **名称**：输入唯一名称。
    - 定价层：标准版 S0
    - **选中此框即表示我确认我已阅读并理解以下所有条款**：已选中。

1. 查看并创建资源，然后等待部署完成。 然后，转到部署的资源。

1. 查看 Azure AI 服务资源的“密钥和终结点”页。 需要终结点和密钥才能从客户端应用程序进行连接。

## 创建自定义视觉项目

若要训练对象检测模型，需要基于训练资源创建自定义视觉项目。 为此，你将使用自定义视觉门户。

1. 在新的浏览器选项卡中，打开自定义视觉门户 ([https://customvision.ai](https://customvision.ai?azure-portal=true))，然后使用与 Azure 订阅关联的 Microsoft 帐户登录。

1. 创建一个具有以下设置的新项目：
    - 名称：交通安全
    - 说明：针对道路安全的物体检测。
    - 资源：先前创建的资源
    - 项目类型：对象检测
    - 域：常规 \[A1]

1. 等待项目创建并在浏览器中打开。

## 添加图像并进行标记

若要训练对象检测模型，需要上传包含希望模型识别的类的图像，并标记这些图像以指示每个对象实例的边界框。

1. 从 [https://aka.ms/traffic-images](https://aka.ms/traffic-images) 下载并提取训练图像。 提取的文件夹包含骑自行车的人和行人图像的集合。

1. 在自定义视觉门户的“交通安全”物体检测项目中，选择“添加图像”，然后将所有图像上传到提取的文件夹。

    ![自定义视觉工作室中“图像上传”对话框的屏幕截图。](media/create-object-detection-solution/upload-images.png)

1. 上传图像后，选择第一个图像将其打开。

1. 将鼠标悬停在图像中的任意物体（骑自行车的人或行人）上，直到显示自动检测到的区域。 然后选择对象，并根据需要调整区域大小以将其环绕。 或者，只需在对象周围拖动以创建区域。

    在矩形区域内严格选择物体时，输入物体（骑自行车的人或行人）的相应标记，并使用标记区域 (+) 按钮将标记添加到项目。

    ![图像的屏幕截图，其中“图像详细信息”对话框中有标记的区域。](media/create-object-detection-solution/tag-image.png)

1. 使用右侧的“下一个”((>) 链接转到下一个图像，并标记其对象 。 然后，继续处理整个图像集合，标记每个骑自行车的人和行人。

    标记图像时，请注意以下事项：

    - 某些图像包含多个物体，这些物体可能属于不同的类型。 标记每一个物体，即使它们重叠也是如此。
    - 输入一次标记后，可以在标记新物体时从列表中选择它。
    - 可以后退和向前浏览图像以调整标记。

    ![图像的屏幕截图，其中“图像详细信息”对话框中有标记的区域。](media/create-object-detection-solution/multiple-objects.png)

1. 标记完最后一个图像后，关闭“图像详细信息”编辑器，在“训练图像”页上的“标记”下，选择“已标记”以查看所有已标记的图像：

    ![项目中标记图像的屏幕截图。](media/create-object-detection-solution/tagged-images.png)

## 训练和测试模型

现在你已标记项目中的图像，即可训练模型。

1. 在自定义视觉项目中，单击“训练”以使用已标记的图像训练对象检测模型。 选择“快速训练”选项。

    > 提示：训练过程可能需要几分钟。 等待时，请查看[智能城市的视频分析](https://www.microsoft.com/research/video/video-analytics-for-smart-cities/)，它描述了在道路安全改善计划中使用计算机视觉的实际项目。

2. 训练完成后，请查看“精度”、“召回率”和“映射”性能指标 - 这些指标度量对象检测模型的预测准确性，并且应该都相当高  。

3. 调整左侧的概率阈值，将其从 50% 增加到 90%，并观察对性能指标的影响。 此设置确定每个标记评估必须满足或超过什么样的概率值才能算作预测。

    ![已训练模型的性能指标的屏幕截图。](media/create-object-detection-solution/performance-metrics.png)

4. 在页面的右上角，单击“快速测试”，然后在“图像 URL”框中输入 `https://aka.ms/pedestrian-cyclist` 并查看结果。

    在右侧窗格中的“预测”下，会列出每个检测到的物体及其标记和概率。 选择每个物体以查看它在图像中突出显示。

    预测的物体可能不是全部正确 - 毕竟，骑自行车的人和行人有许多共同的特征。 模型最有信心的预测具有最高的概率值。 使用“阈值”滑块消除概率较低的物体。 你应该能够找到仅包含正确预测的点（可能为大约 85-90%）。

    ![已训练模型的性能指标的屏幕截图。](media/create-object-detection-solution/test-detection.png)

5. 然后关闭“快速测试”窗口。

## 发布对象检测模型

现在，你已准备好发布已训练模型并从客户端应用程序使用它。

1. 单击“&#128504; 发布”以通过以下设置发布已训练模型：
    - 模型名称：交通安全
    - 预测资源：先前创建的资源。

1. 发布后，单击“预测 URL”(&#127760;) 图标，查看使用已发布模型所需的信息。

    ![预测 URL 的屏幕截图。](media/create-object-detection-solution/prediction-url.png)

稍后，你将需要相应的 URL 和 Prediction-Key 值从图像 URL 中获取预测，因此请保持此对话框处于打开状态，然后继续执行下一个任务。

## 准备客户端应用程序

为了测试自定义视觉服务的功能，我们将使用在 Azure 上的 Cloud Shell 中运行的简单命令行应用程序。

1. 切换回包含 Azure 门户的浏览器选项卡，然后选择搜索框右侧页面顶部的 Cloud shell ([>_]) 按钮 。 随即将在门户底部打开 Cloud Shell 窗格。

    首次打开 Cloud Shell 时，系统可能会提示你选择要使用的 shell 类型（Bash 或 PowerShell）。 如果是这样，选择“PowerShell”。

    如果系统提示你为 Cloud Shell 创建存储，请确保已选中订阅，然后选择“创建存储”。 等待存储创建完毕，此过程大约需要一分钟。

    Cloud Shell 准备就绪后，它应如下所示：
    
    ![Azure 门户中的 Cloud Shell 的屏幕截图。](media/create-object-detection-solution/cloud-shell.png)

    > 提示：确保 Cloud Shell 窗格左上角指示的 shell 类型为 PowerShell。 如果是 Bash，请通过使用下拉菜单切换到 PowerShell。

    请注意，可以通过拖动窗格顶部的分隔条或使用窗格右上角的 &#8212;、&#9723; 或 X 图标来调整 Cloud Shell 的大小，以最小化、最大化和关闭窗格  。 有关如何使用 Azure Cloud Shell 的详细信息，请参阅 [Azure Cloud Shell 文档](https://docs.microsoft.com/azure/cloud-shell/overview)。

2. 在命令行界面中，输入以下命令下载本练习的文件，并将其保存在名为“ai-900”的文件夹中（如果该文件夹已存在，需要先删除该文件夹）

    ```PowerShell
    rm -r ai-900 -f
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

3. 下载文件后，输入以下命令以更改为 ai-900 目录，并编辑本练习的代码文件：

    ```PowerShell
    cd ai-900
    code detect-objects.ps1
    ```

    注意此操作将打开一个编辑器，如下图所示：

     ![Cloud Shell 中代码编辑器的屏幕截图。](media/create-object-detection-solution/code-editor.png)

     > 提示：可以使用 Cloud Shell 命令行和代码编辑器之间的分隔条来调整窗格的大小。

4. 不要太担心代码的详细信息。 重要的是，它从一些代码开始，以指定自定义视觉模型的预测 URL 和密钥。 需要更新这些项，使代码的其余部分使用你的模型。

    从自定义视觉项目的浏览器选项卡中保持打开状态的对话框中获取预测 URL 和预测密钥 。 如果有图像 URL，则需要使用版本。

    使用这些值替换代码文件中的 YOUR_PREDICTION_URL 和 YOUR_PREDICTION_KEY 占位符 。

    粘贴“预测 URL”和“预测密钥”值后，前两行代码应如下所示：

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

5. 对代码中的变量进行更改后，按 Ctrl+S 可保存文件。 然后，按 Ctrl+Q 可关闭代码编辑器。

## 测试客户端应用程序

现在，可以使用示例客户端应用程序来检测图像中的骑自行车的人和行人。

1. 在 PowerShell 窗格中，输入以下命令以运行代码：

    ```PowerShell
    ./detect-objects.ps1 1
    ```

    此代码使用模型检测下图中的物体：

    ![一名行人和一名骑自行车的人的照片。](media/create-object-detection-solution/road-safety-1.jpg)

1. 查看预测，其中列出了检测到的任何概率为 90% 或更高的物体，以及其位置周围的范围框的坐标。

1. 现在，让我们尝试另一张图像。 运行以下命令：

    ```PowerShell
    ./detect-objects.ps1 2
    ```

    这次对下图进行分析：

    ![一群行人的照片。](media/create-object-detection-solution/road-safety-2.jpg)

希望你的物体检测模型能够很好地检测测试图像中的行人和骑自行车的人。

