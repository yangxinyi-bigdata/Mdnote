# 使用Zeppelin学习Spark SQL

## 介绍

在这个由两部分组成的基于实验室的教程中，我们将首先向您介绍Apache Spark SQL。Spark SQL是一个更高级别的Spark模块，允许您对DataFrames和Datasets进行操作，稍后我们将对此进行更详细的介绍。在本教程结束时，我们将为您提供Zeppelin Notebook以导入Zeppelin环境。

在实验的第二部分中，我们将使用高级SQL API探索航空公司数据集。我们将可视化数据集并编写SQL查询，以探索我们预测到达和离开航班最长的延误时间。

该实验室是我们基于Apache Zeppelin的实验系列的一部分，为数据接入，数据处理，数据调整，数据可视化等提供直观且对开发人员友好的基于Web的环境。在本教程中，我们将首先回顾使用的概念，包含本教程中使用的代码的Zeppelin Notebook以及本教程末尾提供的进一步说明。

## 先决条件

- 下载并部署了[Hortonworks数据平台（HDP）](https://zh.hortonworks.com/downloads/#sandbox)沙箱
- [Apache Zeppelin入门](https://zh.hortonworks.com/tutorial/getting-started-with-apache-zeppelin/)
- 掌握[Scala的](http://www.dhgarrette.com/nlpclass/scala/basics.html)基本知识。Zeppelin Notebook使用基本语法
- 回顾[学习HDP Sandbox的绳索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)

## 大纲

- [概念](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#concepts)
- [使用DataFrame和Dataset API分析航空公司数据](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#using-dataframe-and-dataset-api-to-analyze-airline-data)
- [使用SQL API分析航空公司数据](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#using-sql-api-to-analyze-the-airline-data)
- [把它们放在一起](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#putting-it-all-together)
- [导入Zeppelin笔记本](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#import-the-zeppelin-notebook)
- [概要](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#summary)
- [进一步阅读](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/#further-reading)

## 概念

如前所述，这是一个由两部分组成的实验。

在实验的第一部分中，我们将介绍Spark SQL的Datasets and DataFrames，它们是分布式数据集合，在概念上等同于关系数据库中的表格或Python或R中的Dataframe。

两者都提供丰富的优化方法并可以转换为优化的较低级别的Spark代码。

数据集和数据框之间的主要区别在于数据集是强类型的，需要一致的值/变量类型赋值。数据集以Scala和Java（强类型语言）提供，而DataFrame还支持Python和R语言。

如果看不懂，请不要担心。完成本实验后，您会发现Dataset和DataFrame API都提供了与数据交互的直观方式。我们将引导您完成探索和选择相关数据的几个步骤，并创建用户定义函数（UDF）以将基本过滤器应用于感兴趣的列，例如确定哪些航班被延迟。

在实验的第二部分中，我们将创建一个临时视图，将DataFrame存储在内存中，并通过SQL API访问其内容。这将允许我们针对此临时视图运行SQL查询，从而允许使用内置的Zeppelin可视化对数据进行更丰富的探索。

我们将把结果保存到永久表中，然后与其他人共享。

需要记住的一点是，在实验的第一部分和第二部分中，对Datasets/DataFrames或temporary view的查询将转换为基础优化形式的Spark弹性分布式数据集（RDD），确保所有代码都是并行执行的/分布式时尚。要了解有关RDD的更多信息，请参阅[Spark文档](http://spark.apache.org/docs/latest/programming-guide.html#resilient-distributed-datasets-rdds)，这些内容超出了本教程的范围。

## 使用DATAFRAME和DATASET API分析航空公司数据

### Datasets and DataFrames

Dataset是分布式数据集合。数据集是强类型, 因此能够享受强类型的好处，能够使用强大的lambda函数和（Spark SQL）优化执行引擎的优势。数据集可以从JVM对象构造，然后使用功能转换（map，flatMap，filter等）进行操作。数据集API在Scala和Java中可用。

DataFrame是组织为命名列的Dataset。它在概念上等同于关系数据库中的表或R / Python中的Dataframe，但是具有更丰富的优化。DataFrame可以从多种来源构建，例如：结构化数据文件，Hive中的表，外部数据库或现有RDD。DataFrame API在Scala，Java，Python和R中可用。在Scala和Java中，DataFrame由行数据集表示。在Scala API中，DataFrame只是Dataset [Row]的类型别名。（注意，在Scala类型参数（泛型）中用方括号括起来。）

在本文档中，我们经常将行的Scala / Java数据集称为DataFrame。[来源](http://spark.apache.org/docs/2.0.0/sql-programming-guide.html#datasets-and-dataframes)。

### 数据集描述

在本教程中，我们将使用大型飞机飞行记录，包括飞行日期，出发时间，到达时间以及其他数据点，以推断哪些航班可能会延迟，并找出平均延迟时间，这是数据的完整描述：

| **NAME** | **介绍**          |                                                              |
| -------- | ----------------- | ------------------------------------------------------------ |
| 1        | Year              | 1987-2008                                                    |
| 2        | Month             | 1-12                                                         |
| 3        | DayofMonth        | 1-31                                                         |
| 4        | DayOfWeek         | 1 (Monday) – 7 (Sunday)                                      |
| 5        | DepTime           | 实际出发时间actual departure time (local, hhmm)              |
| 6        | CRSDepTime        | 预定出发时间scheduled departure time (local, hhmm)           |
| 7        | ArrTime           | 实际到达时间actual arrival time (local, hhmm)                |
| 8        | CRSArrTime        | 预定抵达时间scheduled arrival time (local, hhmm)             |
| 9        | UniqueCarrier     | 唯一运营商代码unique carrier code                            |
| 10       | FlightNum         | 航班号flight number                                          |
| 11       | TailNum           | 飞机尾号plane tail number                                    |
| 12       | ActualElapsedTime | in minutes                                                   |
| 13       | CRSElapsedTime    | in minutes                                                   |
| 14       | AirTime           | in minutes                                                   |
| 15       | ArrDelay          | 到达延迟，以分钟为单位arrival delay, in minutes              |
| 16       | DepDelay          | 出发延误，以分钟为单位departure delay, in minutes            |
| 17       | Origin            | 起点 origin IATA airport code                                |
| 18       | Dest              | 目的地 destination IATA airport code                         |
| 19       | Distance          | in miles                                                     |
| 20       | TaxiIn            | taxi in time, in minutes                                     |
| 21       | TaxiOut           | taxi out time in minutes                                     |
| 22       | Cancelled         | 航班是否取消 was the flight cancelled?                       |
| 23       | CancellationCode  | reason for cancellation (A = carrier, B = weather, C = NAS, D = security) |
| 24       | Diverted          | 1 = yes, 0 = no                                              |
| 25       | CarrierDelay      | in minutes                                                   |
| 26       | WeatherDelay      | in minutes                                                   |
| 27       | NASDelay          | in minutes                                                   |
| 28       | SecurityDelay     | in minutes                                                   |
| 29       | LateAircraftDelay | in minutes                                                   |

通过在笔记本中使用用户定义函数，你可以找到延迟航班的百分比：

[![百分号延迟航班](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/percent-delayed-flights-800x254.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/percent-delayed-flights.jpg)

我们发现了一些需要的信息; 但是，还可以做得更好。Zeppelin拥有强大的可视化工具，如图形和表格，我们可以使用它们以更具吸引力的格式呈现我们新发现的数据。在本教程的第二部分中，我们将探讨不同的数据呈现方式。

## 使用SQL API分析航空公司数据

如您所见，notebook中的第1部分中显示的数据可以更具交互性。为了获得更加动态的体验，创建了一个临时（内存中）视图，它用于通过表或图形查询数据并与之交互。只要Spark会话处于活动状态，临时视图就允许我们对它执行SQL查询。

以下是本教程的Zeppelin Notebook中使用的临时表的预览：

[![flightsview-TEMP表预览](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/flightsview-temp-table-preview-800x293.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/flightsview-temp-table-preview.jpg)

利用Zeppelin的可视化工具，我们可以比较延迟航班的总数和航空公司的延误时间：

[![flightsview可视化预览](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/flightsview-visualization-preview-800x331.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/flightsview-visualization-preview.jpg)

牛逼！我们找到了我们想要的东西。现在我们已经了解了基础知识，我们可以扩展一些更有用的数据; 例如，我们想知道最佳旅行时间是：

[![延缓每小时](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/delay-per-hour-800x517.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/delay-per-hour.jpg)

## 把以上的东西放在一起

现在，通过本实验第1部分和第2部分中的所有这些基本分析，您应该非常清楚哪些航班延误最多，哪些航线，哪个航班，哪个小时，哪个小时，哪几天和几个月今年，并能够自己开始做出有意义的预测。这就是使用Spark与Zeppelin的强大功能 - 拥有一个强大的环境来执行大型数据集上的数据调整，数据处理，数据可视化等。

### 固化储存结果/数据

最后，让我们通过将DataFrames保存为名为ORC的优化文件格式来保留我们的一些结果。

在我们的Zeppelin Notebook中，我们将DataFrame存储在以下命令中：

```
// Save and Overwrite our new DataFrame to an ORC file
flightsWithDelays.write.format("orc").mode(SaveMode.Overwrite).save("flightsWithDelays.orc")
```

让我们打破这个说法吧。

```
flightsWithDelays.write.format("orc")
```

### 什么是ORC文件格式？

ORC（优化行列）是一种自描述的，类型感知的列式文件格式，专为Hadoop工作负载而设计。它针对大型流式读取进行了优化，但具有快速查找所需行的集成支持。以列式格式存储数据使读者只能读取，解压缩和处理当前查询所需的值。因为ORC文件是类型感知的，所以编写器为类型选择最合适的编码，并在编写文件时构建内部索引。[更多信息在这里。](https://orc.apache.org/)

```
mode(SaveMode.Overwrite)
```

| MODE (SCALA/JAVA)                    | 含义                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| **SaveMode.ErrorIfExists (default)** | 将DataFrame保存到数据源时，如果数据已存在，则会引发异常。    |
| **SaveMode.Append**                  | 将DataFrame保存到数据源时，如果数据/表已存在，则DataFrame的内容应附加到现有数据。 |
| **SaveMode.Overwrite**               | 覆盖模式意味着在将DataFrame保存到数据源时，如果数据/表已经存在，则预期现有数据将被DataFrame的内容覆盖。 |
| **SaveMode.Ignore**                  | 忽略模式意味着在将DataFrame保存到数据源时，如果数据已存在，则预期保存操作不会保存DataFrame的内容并且不会更改现有数据。这类似于SQL中的CREATE TABLE IF NOT EXISTS。 |

## 导入ZEPPELIN笔记本

牛逼！现在您熟悉本教程中使用的概念，并且您已准备好将*Learning Spark SQL*笔记本导入Zeppelin环境。（如果您在任何时候遇到任何问题，请务必[查看Apache Zeppelin入门](https://zh.hortonworks.com/tutorial/getting-started-with-apache-zeppelin/)教程）。

要导入笔记本，请转到Zeppelin主屏幕。

1.单击“ **导入备注”**

2.选择“ **从URL添加”**

3.将以下URL复制并粘贴到**Note URL中**

```
https://raw.githubusercontent.com/hortonworks/data-tutorials/master/tutorials/hdp/learning-spark-sql-with-zeppelin/assets/Learning%20Spark%20SQL.json
```

4.单击“ **import note”**

导入笔记本后，您可以通过以下方式从Zeppelin主屏幕打开它：

5.单击**单击Learning Spark SQL**

启动**Learning to Spark SQL**笔记本后，请按照笔记本中的所有说明完成本教程。

## 概要

完成实验的第一部分和第二部分后，您应该拥有一个基本工具集，可以使用高级编程数据集或DataFrame API或SQL API开始探索新数据集。这两种API都提供相同的性能，同时您可以选择其中一种或两种来完成要求高性能数据探索，争论，重叠和可视化的任务。

## 进一步阅读

- 您可能想要查看有关[Spark机器学习](https://zh.hortonworks.com/tutorial/intro-to-machine-learning-with-apache-spark-and-apache-zeppelin/)的简短入门教程。
- [使用SparkR预测航空公司延误](https://zh.hortonworks.com/tutorial/predicting-airline-delays-using-sparkr/)
- [Hortonworks社区连接（HCC）](https://community.hortonworks.com/spaces/85/data-science.html?type=question)是关于Spark，数据分析/科学以及更多大数据主题的问答的绝佳资源。
- [Hortonworks Apache Spark Docs](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_spark-component-guide/content/ch_developing-spark-apps.html) - 官方Spark文档。
- [Hortonworks Apache Zeppelin Docs](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_zeppelin-component-guide/content/ch_using_zeppelin.html) - 官方Zeppelin文档。



































































