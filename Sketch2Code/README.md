# Sketch2Code Documentation

## Description
Sketch2Code is a solution that uses AI to transform a handwritten user interface design from a picture to valid HTML markup code. 

## Process flow
The process of transformation of a handwritten image to HTML this solution implements is detailed as follows:
1.	The user uploads an image through the website.
2.	A custom vision model predicts what HTML elements are present in the image and their location.
3.	A handwritten text recognition service reads the text inside the predicted elements.
4.	A layout algorithm uses the spatial information from all the bounding boxes of the predicted elements to generate a grid structure that accommodates all.
5.	An HTML generation engine uses all these pieces of information to generate an HTML markup code reflecting the result.
1.用户通过网站上传图像。
2.自定义视觉模型预测图像中存在的HTML元素及其位置。
3.手写文本识别服务读取预测元素内的文本。
4.布局算法使用来自预测元素的所有边界框的空间信息来生成适应所有边界框的网格结构。
5. HTML生成引擎使用所有这些信息来生成反映结果的HTML标记代码。

## Architecture
The Sketch2Code solution uses the following elements:
-	A Microsoft Custom Vision Model: This model has been trained with images of different handwritten designs tagging the information of most common HTML elements like Buttons, TextBox and Image.
-	A Microsoft Computer Vision Service: To identify the text written into a design element a Computer Vision Service is used.
-	An Azure Blob Storage: All steps involved in the HTML generation process are stored, including original image, prediction results and layout grouping information. 
-	An Azure Function: Serves as the backend entry point that coordinates the generation process by interacting with all the services.
-	An Azure WebSite: User font-end to enable uploading a new design and see the generated HTML results.
This elements form the architecture as follows:
![Sketch2Code Architecture](https://github.com/Microsoft/ailab/blob/master/Sketch2Code/images/architecture.png)

Sketch2Code解决方案使用以下元素：
- Microsoft自定义视觉模型：此模型已经过训练，其中包含不同手写设计的图像，用于标记最常见的HTML元素（如按钮，文本框和图像）的信息。
- Microsoft计算机视觉服务：为了识别写入设计元素的文本，使用计算机视觉服务。
- Azure Blob存储：存储HTML生成过程中涉及的所有步骤，包括原始图像，预测结果和布局分组信息。
- Azure功能：用作后端入口点，通过与所有服务交互来协调生成过程。
- Azure WebSite：User font-end，用于上载新设计并查看生成的HTML结果。
这些元素构成如下架构：


## How to configure the solution

### Microsoft Custom Vision Model
The training set used to create the sample model used in the project is located in the Model folder. Each training image has a unique identifier that matches information contained in the dataset.json file. This file contains all the tag information used to train the sample model.
To create your own model you can use this dataset to start and using the Custom Vision API upload this dataset to your own project.
You can create your Custom Vision Project at https://customvision.ai
Once you have created your Custom Vision Project you need to annotate the Key and the Project Name to configure the Azure Function to call use the service.

### Microsoft自定义视觉模型
用于创建项目中使用的样本模型的训练集位于“模型”文件夹中。每个训练图像都有一个唯一的标识符，该标识符与dataset.json文件中包含的信息相匹配。此文件包含用于训练样本模型的所有标记信息。
要创建自己的模型，您可以使用此数据集启动并使用Custom Vision API将此数据集上载到您自己的项目。
您可以通过https://customvision.ai创建自定义视觉项目
创建自定义视觉项目后，需要注释密钥和项目名称以配置Azure功能以调用使用该服务

### Computer Vision Service
A Microsoft Computer Vision Service is needed to perform handwritten character recognition.
To create this service go to your Azure Subscription and create your own service. Annotate the Endpoint and the Key to complete the configuration of the solution.
###计算机视觉服务
需要Microsoft Computer Vision Service来执行手写字符识别。
要创建此服务，请转到Azure订阅并创建自己的服务。注释端点和密钥以完成解决方案的配置。

### Azure Blob Storage
An Azure Blob Storage is used to store all the intermediary steps of the process.
A new folder is created for each generation process with the following contents:
-	\slices: Contains the cropped images used for text prediction.
-	Original.png: image uploaded by the user.
-	results.json: results from the prediction process run against the original image.
-	groups.json: results from the layout algorithm containing the spatial distribution of predicted objects.
### Azure Blob存储
Azure Blob存储用于存储该进程的所有中间步骤。
将为每个生成过程创建一个新文件夹，其中包含以下内容：
- \ slices：包含用于文本预测的裁剪图像。
- Original.png：用户上传的图片。
- results.json：预测过程的结果针对原始图像运行。
- groups.json：包含预测对象的空间分布的布局算法的结果。

### Azure function
The code for the provided Azure Function is located in the Sketch2Code.Api folder. You can use this code to create your own function and must define the following configuration parameters:
-	AzureWebJobsStorage: Endpoint to the Azure Storage.
-	ComputerVisionDelay: Time in milliseconds to wait between calls to the computer vision service. Sample works with 120ms.
-	HandwrittenTextApiEndpoint: Endpoint to the Computer Vision Service.
-	HandwrittenTextSubscriptionKey: Key to access the Computer Vision Service.
-	ObjectDetectionIterationName: Name of the training iteration to use in the prediction model.
-	ObjectDetectionPredictionKey: Prediction Key to access the Custom Vision Service.
-	ObjectDetectionProjectName: Name of the custom vision project.
-	ObjectDetectionTrainingKey: Training key for the custom vision service.
-	Probability: Probability threshold to consider a successful object detection. Below this value predictions are not considered. Sample model works with 40. 
### Azure功能
提供的Azure功能的代码位于Sketch2Code.Api文件夹中。您可以使用此代码创建自己的函数，并且必须定义以下配置参数：
- AzureWebJobsStorage：Azure存储的端点。
- ComputerVisionDelay：在呼叫计算机视觉服务之间等待的时间（以毫秒为单位）。样本工作120ms。
- HandwrittenTextApiEndpoint：计算机视觉服务的端点。
- HandwrittenTextSubscriptionKey：访问计算机视觉服务的密钥。
- ObjectDetectionIterationName：要在预测模型中使用的训练迭代的名称。
- ObjectDetectionPredictionKey：用于访问自定义视觉服务的预测键。
- ObjectDetectionProjectName：自定义视觉项目的名称。
- ObjectDetectionTrainingKey：自定义视觉服务的培训密钥。
- 概率：考虑成功检测对象的概率阈值。低于此值预测不予考虑。样本模型适用于40。

### Azure Website
The Sketch2Code.Web contains the code used to implement the front-end website. Two parameters must be configured:
-	Sketch2CodeAppFunctionEndPoint: Endpoint to the Azure Function
-	storageUrl: Url for the Azure Storage. 
### Azure网站
Sketch2Code.Web包含用于实现前端网站的代码。必须配置两个参数：
- Sketch2CodeAppFunctionEndPoint：Azure函数的端点
- storageUrl：Azure存储的URL。


