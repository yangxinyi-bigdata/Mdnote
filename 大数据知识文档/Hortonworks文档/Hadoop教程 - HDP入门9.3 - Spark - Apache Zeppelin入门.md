# Apache Zeppelin入门

## 介绍

Apache Zeppelin是一款基于Web的notebook，支持交互式数据分析。使用Zeppelin，您可以使用丰富的预构建语言后端（或解释器）制作精美的数据驱动，交互式和协作文档，例如Scala（使用Apache Spark），Python（使用Apache Spark），SparkSQL，Hive ，Markdown，Angular和Shell。

Zeppelin专注于商业工作，具有以下重要功能：

- Livy集成（用于与Spark交互的REST接口）
- 安全：
  - 只有经过身份验证的用户才能执行作业
  - 针对LDAP的Zeppelin身份验证
  - Notebook授权

本教程的目的是指导您完成Zeppelin的基本功能，以便您可以创建自己的数据分析应用程序或导入现有的Zeppelin Notebook; 此外，您将学习Zeppelin的高级功能，如创建和绑定解释器，以及导入外部库。

## 先决条件

- 安装并部署了[最新的HDP Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- 确保已安装Spark版本2.x或更高版本

## 大纲

- Launching Zeppelin
- Creating a Notebook
- Importing a Notebook
- Deleting a Notebook
- Adding a Paragraph
- Running a Paragraph
- Clearing Paragraph Output
- Creating an Interpreter
- Binding an Interpreter
- Exporting a Notebook
- Importing External Libraries
- Summary
- Further Reading

## 概念

本教程是将在未来的Spark教程中使用的基础，并涵盖了重要的主题，如创建笔记本，导入和扩展现有笔记本，以及将不同的后端绑定到您的环境，以便您可以使用Zeppelin充分发挥它的潜力。

## 运行ZEPPELIN

在HDP环境中有两种方式可以访问Zeppelin，第一种是通过Amabari的**快速链接**，第二种是通过浏览器导航到Zeppelin的专用端口。

### 通过AMBARI启动ZEPPELIN

使用**amy_ds / amy_ds**作为用户名/密码组合登录**Ambari**（操作控制台）。

> 注意：回想一下，当Sandbox运行时，浏览器上的Ambari可以通过**http://sandbox-hdp.hortonworks.com:8080**访问。如果您是HDP Sandbox环境的新手，请务必查看[学习HDP Sandbox的绳索。](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)

[![登录的sCR1](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/scr1-login-3-800x505.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/scr1-login-3.jpg)

进入Ambari后，单击Zeppelin Notebook，然后单击快速链接，最后点击Zeppelin UI。

[![获取到策普](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/getting-to-zepp-3-800x457.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/getting-to-zepp-3.jpg)

瞧，您应该看到默认的Zeppelin菜单。

[![SCR3  - 齐柏林主](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/welcome-to-zepp-3-800x426.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/welcome-to-zepp-3.jpg)

### 从URL启动ZEPPELIN

启动Zeppelin实例的第二个选项是打开浏览器并在沙盒运行时导航到**http://sandbox-hdp.hortonworks.com:9995**。

> 注意：要使此方法起作用，您必须已在**主机**文件中重命名了沙盒IP地址。如果您需要帮助重命名主机文件中的默认IP，请访问[HDP Sandbox Tutorial](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#map-sandbox-ip-to-your-desired-hostname-in-the-hosts-file)的[Learning the Ropes](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#map-sandbox-ip-to-your-desired-hostname-in-the-hosts-file)

[![欢迎到策普 - 网址](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/welcome-to-zepp-url-3-800x480.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/welcome-to-zepp-url-3.jpg)

现在让我们创建你的第一个笔记本。

## 创建一个笔记本

要创建笔记本：

1.在“笔记本”选项卡下，选择“ **创建新笔记”**。

[![单击创建新音符](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/click-create-new-note-3-800x426.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/click-create-new-note-3.jpg)

2.您将看到以下窗口。输入新笔记的名称（在本例中我们将其称为*HDP上的Spark*），现在让我们将解释器选项保留为默认设置，您将在后续章节中了解解释器。最后，点击“创建”。

[![创建全新的音符 - 策普](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/create-new-note-zepp-3.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/create-new-note-zepp-3.jpg)

默认情况下，新笔记本将打开一个空白段落，如果您想稍后再回来处理它，您将在主Zeppelin UI上找到您的笔记本。

[![新的音符，保存](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/new-note-saved-3-800x422.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/new-note-saved-3.jpg)

好的，现在您知道如何从头开始创建笔记本了。在编写代码之前，让我们了解导入已存在的Zeppelin笔记本的不同方法。

## 导入笔记本

您可能希望导入现有笔记本，而不是创建新笔记本。

有两种方法可以导入Zeppelin笔记本，可以通过指向您环境本地的json笔记本文件，也可以通过提供一个url到其他地方托管的原始文件，例如在github上。我们将介绍导入这些文件的两种方式。

1.导入JSON文件

在Zeppelin UI上单击“导入”。

[![进口音符](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-note-3-800x426.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-note-3.jpg)

接下来，单击“ **选择JSON文件”**按钮。

[![进口从-JSON-策普](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-json-zepp-3-800x422.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-json-zepp-3.jpg)

最后，选择要导入的笔记本，然后单击“ **打开”**。

[![进口从JSON的选档](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-json-select-file-3.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-json-select-file-3.jpg)

现在，您应该在Zeppelin主屏幕上的其他笔记本中看到您导入的笔记本。

2.使用URL导入Notebook

单击导入注释。

[![进口音符](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-note-3-800x426.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-note-3.jpg)

接下来，单击“ **从URL添加”**按钮。

[![进口从-URL-策普](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-url-zepp-3-800x422.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-url-zepp-3.jpg)

最后，将url粘贴到（原始）json文件，然后单击**Import Note**。

[![进口从-URL-策普贴](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-url-zepp-paste-3-800x426.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/import-from-url-zepp-paste-3.jpg)

现在，您应该在Zeppelin主屏幕上的其他笔记本中看到您导入的笔记本。

## 删除笔记本

如果您想删除笔记本，可以转到Zeppelin欢迎页面。在**笔记本**下面的页面左侧，您将看到各种选项，例如**导入笔记**，**创建新笔记**，**过滤**框，在**过滤**框下方，您可以在其中找到您创建或导入的笔记本。

删除笔记本（参见下面的图片作为参考）

1.将鼠标悬停在要删除的笔记本上

将出现各种图标，包括垃圾桶。

2.单击垃圾箱图标

[![deleting_a_notebook](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/deleting_a_notebook-3-800x322.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/deleting_a_notebook-3.jpg)

提示会询问您是否要**将此笔记移至垃圾箱？**，单击“ **确定”**。

笔记本电脑现已移至Zeppelin的垃圾箱。您可以随时通过单击“ **废纸篓”**文件夹还原笔记本，将鼠标悬停在要还原的笔记本上，在那里您将看到两个选项：弯曲箭头，其中显示“ **还原备注”**和允许您永久删除项目的**x**。如果要恢复笔记本，请单击**恢复注释**，然后单击**x**以从Zeppelin中永久删除该项目。

## 添加段落

到现在为止，您一定已经渴望开始编程或扩展您的notebook。在Zeppelin中添加一个段落非常简单。首先打开要处理的笔记本。

接下来，将鼠标悬停在现有段落的下边缘或上边缘上，您将看到添加段落的选项。

[![附加段落悬停以上](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/add-paragraph-hover-above-3-800x228.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/add-paragraph-hover-above-3.jpg)

单击上方以在现有段落之前添加新段落。

[![附加段落悬停以下](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/add-paragraph-hover-below-3-800x228.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/add-paragraph-hover-below-3.jpg)

或者单击下方以在当前段落之后添加新段落。

好的，既然您已创建或导入笔记本并编写了段落，则下一步是运行段落。

## 运行段落

在Zeppelin笔记本中有两种运行段落的方法，步骤1和2介绍了如何运行单个段落。第3步将向您展示如何通过一次单击所有段落。

1.单击段落右侧的播放按钮（蓝色）三角形或

2.按**Shift + Enter**

[![运行段落](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/running_a_paragraph-3-800x115.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/running_a_paragraph-3.jpg)

3.单击Zeppelin笔记本顶部的播放按钮（蓝色）三角形，如下图所示。

[![运行段落](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/running_paragraphs-3-800x115.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/running_paragraphs-3.jpg)

## 清除段落输出

要清除特定段落的输出：

1.单击位于段落最右侧的齿轮。

2.选择**清除输出**

[![清除段落的输出](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/clear_a_paragraph_output-3-800x305.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/clear_a_paragraph_output-3.jpg)

这将清除该特定段落的输出

要清除整个Zeppelin笔记本的输出，请转到笔记本顶部并选择橡皮擦图标。将出现一个提示，询问*您是否要清除所有输出？*按**确定**。

[![清除所有段落的输出](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/clear_all_outputs-3-800x111.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/clear_all_outputs-3.jpg)

## 创建一个解释器

Zeppelin笔记本支持各种解释器，允许您对数据执行许多操作。以下是您可以使用Zeppelin解释器执行的一些操作：

- Ingestion
- 改写（munging）
- Wrangling
- Visualization可视化
- Analysis分析
- Processing处理

这些是将在我们各种Spark教程中使用的一些解释器。

| 翻译员           | 介绍                                            |
| ---------------- | ----------------------------------------------- |
| **%spark2**      | Spark解释器运行用Scala编写的Spark 2.x代码       |
| **％spark2.sql** | Spark SQL解释器（对Spark中的临时表执行SQL查询） |
| **%sh**          | Shell解释器运行shell命令，如移动文件            |
| **%angular**     | 用于运行Angular和HTML代码的Angular解释器        |
| **%md**          | 用于显示格式化文本，链接和图像的Markdown        |

请注意每个解释器开头的**％**。每个段落都需要以**％**开头，后跟解释器名称。下图显示了三个解释器，Markdown，Spark和Shell。

[![口译员的例子 ](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/interpreter_examples-3-800x373.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/interpreter_examples-3.jpg)

在Zeppelin中创建一个解释器：

1. 单击位于Zeppelin欢迎页面右侧的**用户名**

2. 在下拉列表中选择**Interpreter**

[![创建口译员第1部分](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/creating_an_interpreter_1-3-800x337.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/creating_an_interpreter_1-3.jpg)

3. 在Interpreters页面的右上角，您将看到**Create**，单击它

[![创建口译员第2部分](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/creating_an_interpreter_2-3-800x308.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/creating_an_interpreter_2-3.jpg)

这将打开**Create new interpreter**选项。我们将使用shell解释器作为示例。

4. 在“ **解释器名称”**框中键入**sh**

5. 在 Interpreter Group键入sh

6. 单击“ **保存”**

[![创建口译员第3部分](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/creating_an_interpreter_3-3-800x454.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/creating_an_interpreter_3-3.jpg)

一旦你完成创建解释器，你需要将它绑定到你将使用它的笔记本。下一节将介绍如何将解释器绑定到笔记本中。

## 绑定解释器

要绑定刚刚创建的解释器，您需要重新打开要绑定新解释器的笔记本。

1.单击Zeppelin笔记本右上角的齿轮。请注意，当您单击该齿轮时，它会显示**解释器绑定**

设置部分出现，你可以看到新创建的解释，在我们的情况下，命令解释程序**SH**。

2.单击解释器，它将从白色变为蓝色。

3.单击“ **保存”**

您的新shell解释器已准备好投入使用。

[![译员绑定](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/binding_interpreter_example-3-800x397.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/binding_interpreter_example-3.jpg)

## 导出笔记本

要导出您正在使用的笔记本电脑，只需在您正在使用的笔记本电脑的顶部即可。

1.单击下图中显示的下载图标：

[![导出笔记本](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/export_notebook-2-800x120.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/export_notebook-2.jpg)

这会将笔记本作为JSON文件下载到本地计算机中。

## 导入外部库

当您探索Zeppelin时，您可能希望使用一个或多个外部库。例如，要运行[Magellan，](http://zh.hortonworks.com/blog/magellan-geospatial-analytics-in-spark/)您需要导入其依赖项; 您需要在您的环境中包含Magellan库。在Zeppelin笔记本中有三种方法可以包含外部依赖项：

1.使用**％dep**解释器（**注意**：这仅适用于发布到Maven的库。）

```
%dep
z.load("group:artifact:version")
```

2.使用**％spark2**解释器

```
%spark2
```

3.使用**import**语句

```
import ...
```

以下是使用**％dep**解释器导入Magellan依赖项的示例：

```
%dep
z.addRepo("Spark Packages Repo").url("http://dl.bintray.com/spark-packages/maven")
z.load("com.esri.geometry:esri-geometry-api:1.2.1")
z.load("harsha2010:magellan:1.0.3-s_2.10")
```

## 概要

恭喜！您现在知道Zeppelin的基本功能。现在您可以创建，导入，删除和运行Zeppelin Notebook。此外，您知道如何创建和绑定解释器，导出笔记本和导入外部库。

我们希望我们能够让您感兴趣并且非常兴奋，可以使用Zeppelin进一步探索Spark。请务必查看其他教程，了解更多深入的Spark SQL模块示例，以及用于Streaming和/或Machine的其他Spark模块学习任务。

## 进一步阅读

- [Spark概述](https://spark.apache.org/docs/latest/)
- [Apache Zeppelin](https://zeppelin.apache.org/)
- [Hortonworks社区连接](https://community.hortonworks.com/index.html)











































































































































































