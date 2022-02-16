## 本地开发 

如果你在本地计算机上工作，可以按照以下步骤配置环境以使用实验室。  

### C++ 可再发行程序包 
1. 下载并安装 [Visual C++ 可再发行程序包 (x64)](https://aka.ms/vs/16/release/vc_redist.x64.exe) 

### Python 和需要的包 
1. 安装 [Python 3.6.1](https://www.python.org/downloads/release/python-361/)  
   - **重要事项**： 选择将 Python 添加到 PATH 变量并注册为默认 Python 环境的选项 
2. 安装完成后，打开 *命令提示* 符并输入以下命令以安装必需的包： 

> pip install ipython jupyter matplotlib pillow requests azure-cognitiveservices-vision-computervision azure-cognitiveservices-vision-customvision azure-cognitiveservices-vision-face azure-cognitiveservices-language-textanalytics azure.cognitiveservices.speech azure_ai_formrecognizer 

### Visual Studio Code 
1. 如果尚未安装 Visual Studio Code，[请在此处下载](https://code.visualstudio.com/Download)。安装后，启动 Visual Studio Code 并在“扩展”选项卡 (CTRL+SHIFT+X) 上搜索并安装 Microsoft 提供的 **Python** 扩展。

2. 在 Visual Studio Code 中打开一个新的终端，键入 **git clone https://github.com/MicrosoftLearning/AI-900ZH-Microsoft-Azure-AI-Fundamentals** 并按 **Enter**。 

