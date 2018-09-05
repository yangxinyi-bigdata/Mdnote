# 在5分钟内动手游览Apache Spark

## 介绍

在本教程中，我们将概述Apache Spark，它与Scala，Zeppelin笔记本，解释器，数据集和DataFrame的关系。最后，我们将为我们的开发环境展示[Apache Zeppelin](https://zeppelin.apache.org/)笔记本，以保持简洁和优雅。

Zeppelin将允许我们在预先配置的环境中运行，并执行为Scala和SQL中的Spark编写的代码，一些基本的Shell命令，预先写好的Markdown方向和HTML格式的表。

为了让事情变得有趣和有趣，我们将介绍[硅谷喜剧电视节目](https://www.imdb.com/title/tt2575988/)的电影系列数据集，并在Zeppelin中使用Spark执行一些基本操作。

## 先决条件

- 下载并安装最新的[Hortonworks数据平台（HDP）沙箱](https://zh.hortonworks.com/downloads/#sandbox)
  - 您将需要专用于虚拟机的**8GB**内存，这意味着您的系统应至少有**12GB**的内存。
- [学习HDP沙箱的绳索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- 基本[Scala](http://www.dhgarrette.com/nlpclass/scala/basics.html)语法
- 安装[Zeppelin Shell](https://zh.hortonworks.com/tutorial/getting-started-with-apache-zeppelin/#installing-zeppelin-shell)

## 大纲

- [概念](https://zh.hortonworks.com/tutorial/hands-on-tour-of-apache-spark-in-5-minutes/#concepts)
- [Apache Spark在5分钟内笔记本概述](https://zh.hortonworks.com/tutorial/hands-on-tour-of-apache-spark-in-5-minutes/#apache-spark-in-5-minutes-notebook-overview)
- [在5分钟笔记本中导入Apache Spark](https://zh.hortonworks.com/tutorial/hands-on-tour-of-apache-spark-in-5-minutes/#import-the-apache-spark-in-5-minutes-notebook)
- [概要](https://zh.hortonworks.com/tutorial/hands-on-tour-of-apache-spark-in-5-minutes/#summary)
- [进一步阅读](https://zh.hortonworks.com/tutorial/hands-on-tour-of-apache-spark-in-5-minutes/#further_reading)

## 概念

### APACHE SPARK

Apache Spark是一种快速的内存数据处理引擎，在Scala，Java，Python和R中具有优雅且富有表现力的开发API，允许开发人员执行各种数据密集型工作负载。

[![Spark Logo](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/spark-logo.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/spark-logo.png)

Spark数据集是从各种来源创建的强类型分布式数据集合：JSON和XML文件，Hive中的表，外部数据库等。从概念上讲，它们相当于关系数据库中的表或R或Python中的DataFrame。

### SCALA新手？

在本教程中，我们将使用基本的Scala语法。

了解有关Scala的更多信息，这是一个很好的[入门教程](http://www.dhgarrette.com/nlpclass/scala/basics.html)。

### ZEPPELIN新手？

如果您还没有，请查看Hortonworks [Apache Zeppelin](https://zh.hortonworks.com/apache/zeppelin/)页面以及[Apache Zeppelin入门](https://zh.hortonworks.com/tutorial/getting-started-with-apache-zeppelin/)教程。你会在这里找到官方的Apache Zeppelin页面。

### SPARK新手？

Apache Spark是一种快速的内存数据处理引擎，具有优雅且富有表现力的开发API，允许数据工作者有效地执行需要快速迭代访问数据集的流，机器学习或SQL工作负载。

如果您想了解有关Apache Spark的更多信息，请访问：

- [官方Apache Spark页面](http://spark.apache.org/)
- [Hortonworks Apache Spark Page](https://zh.hortonworks.com/apache/spark/)
- [Hortonworks Apache Spark Docs](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html)

### 什么是解析器？

Zeppelin笔记本支持各种解释器，允许您对数据执行许多操作。以下是您可以使用Zeppelin解释器执行的一些操作：

- Ingestion
- 改写（munging）
- Wrangling
- Visualization/可视化
- Analysis/分析
- Processing/处理

这些是将在我们各种Spark教程中使用的一些解释器。

| 翻译员           | 介绍                                            |
| ---------------- | ----------------------------------------------- |
| **％spark2**     | Spark解释器运行用Scala编写的Spark 2.x代码       |
| **％spark2.sql** | Spark SQL解释器（对Spark中的临时表执行SQL查询） |
| **%sh**          | Shell解释器运行shell命令，如移动文件            |
| **%angular**     | 用于运行Angular和HTML代码的Angular解释器        |
| **%md**          | 用于显示格式化文本，链接和图像的Markdown        |

请注意每个解释器开头的**％**。每个段落都需要以**％**开头，后跟解释器名称。

[了解有关Zeppelin解释器的更多信息](https://zeppelin.apache.org/docs/0.5.6-incubating/manual/interpreters.html)。

### 什么是DATASETS AND DATAFRAMES？

Datasets和DataFrame是从各种来源创建的分布式数据集合：JSON和XML文件，Hive中的表，外部数据库等。从概念上讲，它们相当于关系数据库中的表或R或Python中的DataFrame。数据集和DataFrame之间的主要区别在于数据集是强类型的。

数据集和数据框架上可能存在复杂的操作，但它们超出了本快速指南的范围。

[详细了解数据集和数据框架](http://spark.apache.org/docs/2.0.0/sql-programming-guide.html#datasets-and-dataframes)。

## 5分钟记录APACHE SPARK

[![硅谷图片](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/silicon_valley_corporation-3-800x374.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/silicon_valley_corporation-3.jpg)

我们将有关硅谷Show剧集的外部数据集下载并摄取到Spark数据集中，并执行基本分析，过滤和字数统计。

在应用于数据集的一系列转换之后，我们将定义一个临时视图（表），如下面的那个。

[![DataFrame内容表](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/dataframe_contents_table-2-800x283.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/dataframe_contents_table-2.jpg)

您将能够通过SQL查询来探索这些表，如下所示。

[![复杂的SQL查询图](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/complex_sql_query_graph-2-800x314.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/complex_sql_query_graph-2.jpg)

一旦掌握了数据并执行基本单词计数，我们将为更复杂的字数统计分析添加更多步骤，如下所示。

[![改进的字数计数样本](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/improved_word_count_sample-2-800x227.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/improved_word_count_sample-2.jpg)

在本教程结束时，您应该对Spark有一个基本的了解，并对其强大而富有表现力的API有所了解，并且还有开发人员友好的Zeppelin笔记本环境。

## 导入note

将*5分钟*笔记本中的*Apache Spark*导入您的Zeppelin环境。（如果您在任何时候遇到任何问题，请务必[查看Apache Zeppelin入门](https://zh.hortonworks.com/tutorial/getting-started-with-apache-zeppelin/)教程）。

要导入笔记本，请转到Zeppelin主屏幕。

1. 单击“ **导入note”**

2. 选择“ **从URL添加”**

3. 将以下URL复制并粘贴到**Note URL中**

```
# 5分钟notebook入门ApacheSpark

https://raw.githubusercontent.com/hortonworks/data-tutorials/master/tutorials/hdp/hands-on-tour-of-apache-spark-in-5-minutes/assets/Getting%20Started%20_%20Apache%20Spark%20in%205%20Minutes.json
```

4. 单击“ **导入note”**

   导入笔记本后，您可以通过以下方式从Zeppelin主屏幕打开它：

5. 单击“ **Getting Started”**

6. Select Apache Spark in 5 Minutes

当**Apache Spark在5分钟**notebook 中启动，请按照notebook 中的所有说明完成教程。

## 总结

我们希望您能够成功运行这个简短的入门笔记本，我们已经让您感兴趣并且非常兴奋，可以通过Zeppelin进一步探索Spark。

## 进一步阅读

- [Hortonworks Apache Spark教程](https://zh.hortonworks.com/tutorials/?filters=apache-spark)是您自然而然的下一步，您可以更深入地探索Spark。
- [Hortonworks社区连接（HCC）](https://community.hortonworks.com/spaces/85/data-science.html?type=question)是关于Spark，数据分析/科学以及更多大数据主题的问答的绝佳资源。
- [Hortonworks Apache Spark Docs](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html) - 官方Spark文档。
- [Hortonworks Apache Zeppelin Docs](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/zeppelin-overview/content/overview.html) - 官方Zeppelin文档。