## 使用ZEPPELIN进行数据报告

## 介绍

在本教程中，您将了解Apache Zeppelin并教您使用Zeppelin可视化数据。

## 先决条件

本教程是使用Hortonworks沙箱开始使用HDP的系列教程的一部分。在继续本教程之前，请确保完成先决条件。

- 已下载并已安装[Hortonworks Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- [学习HDP沙箱的线索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- [将传感器数据加载到HDFS中](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/2/)
- [Hive - 数据ETL](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/)
- [Pig - 风险因素](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/)
- [Spark - 风险因素](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/)

## 大纲

- [Apache Zeppelin](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/#apache-zeppelin)
- [创建一个Zeppelin笔记本](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/#create-a-zeppelin-notebook)
- [执行Hive查询](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/#execute-a-hive-query)
- [使用Zeppelin构建图表](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/#build-charts-using-zeppelin)
- [概要](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/#summary)
- [进一步阅读](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/#further-reading)



## APACHE ZEPPELIN

Apache Zeppelin为数据分析和挖掘提供了一个功能强大的基于Web的笔记本平台。 它支持Spark分布式上下文以及Spark之上的其他语言绑定。

在本教程中，我们将使用Apache Zeppelin对 我们之前收集的地理定位，卡车和风险因素数据(geolocation, trucks, and riskfactor data)运行SQL查询，并通过图形和图表可视化结果。

## 创建一个ZEPPELIN笔记本

### 导航到ZEPPELIN NOTEBOOK

```
http://sandbox-hdp.hortonworks.com:9995/
```

![zeppelin_welcome_page_hello_hdp_lab4](pictures/107)



单击左上角的Notebook选项卡，然后选择**Create new note**。为笔记本命名。`Driver Risk Factor`

![zeppelin_create_new_notebook](pictures/108)

## 执行HIVE查询

### 以表格格式显示最终结果数据

在之前的Spark和Pig教程中，您已经创建了一个表，`finalresults`或者`riskfactor`提供了与每个驱动程序相关的风险因素。我们将使用此表中生成的数据来可视化哪些驱动因素具有最高风险因素。我们将使用jdbc Hive解释器在Zeppelin中编写查询。

1. 将下面的代码复制并粘贴到您的Zeppelin笔记中。

```sql
%jdbc(hive)
SELECT * FROM riskfactor
```

1. 单击“ready”或“finished”旁边的播放按钮，在Zeppelin笔记本中运行查询。
   运行查询的另一种方法是“shift + enter”。

最初，查询将以表格格式生成数据，如屏幕截图所示。

![output_riskfactor_zeppelin_lab6](pictures/109)

## 使用ZEPPELIN构建图表

### 以图表格式显示最终结果数据

1.遍历查询下方显示的每个选项卡。
每个图表将根据查询中返回的数据显示不同类型的图表。

![charts_tab_under_query_lab6](pictures/110)

2.点击图表后，我们可以查看额外的高级设置，以定制我们想要的数据视图。

![bar_graph_zeppelin_lab6](pictures/111)

3.单击设置以打开高级图表功能。

4.要使用和创建图表，请将表格关系拖动到框中，如下图所示。`riskfactor.driverid``riskfactor.riskfactor SUM`

![fields_set_keys_values_chart_lab6](pictures/112)

你现在应该看到如下图所示的图像。

![driverid_riskfactor_chart_lab6](pictures/113)

6.如果您将鼠标悬停在峰值上，则每个峰值都会给出驱动因素和风险因素。

![hover_over_peaks_lab6](pictures/114)

7.尝试尝试不同类型的图表以及拖放不同的表字段以查看可以获得的结果类型。

8.让我们尝试不同的查询，找出哪些城市和州包含风险因素最高的司机。

```sql
%jdbc(hive)
SELECT a.driverid, a.riskfactor, b.city, b.state
FROM riskfactor a, geolocation b where a.driverid=b.driverid
```

![queryFor_cities_states_highest_driver_riskfactor](pictures/115)

9.在更改了一些设置后，我们可以确定哪些城市具有高风险因素。
尝试通过单击**散点图**图标来更改图表设置。然后确保键a.driverid 
在xAxis字段中，a.riskfactor在yAxis字段中，b.city在group字段中。
该图表应类似于以下内容。

![visualize_cities_highest_driver_riskfactor_lab6](pictures/116)

您可以将鼠标悬停在最高点以确定哪个驱动因素具有最高风险因素以及哪些城市。

## 概要

现在我们知道如何使用Apache Zeppelin来获取和可视化我们的数据，我们也可以使用
我们从Hive，Pig和Spark实验室学到的技能，并将它们应用于新类型的数据
，以便更好地实现从数字的意义和意义！

## 进一步阅读

- [Zeppelin on HDP](https://zh.hortonworks.com/hadoop/zeppelin/)
- [Apache Zeppelin Docs](https://zeppelin.incubator.apache.org/docs/)
- [Zeppelin主页](https://zeppelin.incubator.apache.org/)

















