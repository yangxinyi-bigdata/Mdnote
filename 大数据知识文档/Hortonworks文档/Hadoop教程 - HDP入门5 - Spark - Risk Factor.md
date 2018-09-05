## Spark - 风险因素

## 介绍

在本教程中，我们将介绍Apache Spark。在本实验的前面部分中，您学习了如何将数据加载到HDFS中，然后使用Hive对其进行操作。我们正在使用卡车传感器数据来更好地了解与每个驾驶员相关的风险。本节将教您如何使用Apache Spark计算风险。

## 先决条件

本教程是使用Hortonworks沙箱开始使用HDP的一系列教程的一部分。在继续本教程之前，请确保完成先决条件。

- 已下载并已安装[Hortonworks Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- [学习HDP沙箱的线索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- [将传感器数据加载到HDFS中](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/2/)
- [Hive - 数据ETL](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/)



## 大纲

- [概念](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#concepts)
- [Apache Spark基础知识](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#apache-spark-basics)
- [使用Ambari配置Spark服务](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#configure-spark-services-using-ambari)
- [创建一个Hive上下文](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#create-a-hive-context)
- [从Hive Context创建RDD](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#create-a-rdd-from-hive-context)
- [查询表](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#querying-against-a-table)
- [将数据作为ORC加载并保存到Hive中](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#load-and-save-data-into-hive-as-orc)
- [完整的Spark代码审查](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#full-spark-code-review)
- [概要](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#summary)
- [进一步阅读](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#further-reading)
- [附录A：在Spark Interactive Shell中运行Spark](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#run-spark-in-shell)

## 概念

MapReduce非常有用，但作业运行所需的时间有时可能是非常漫长的。此外，MapReduce作业仅适用于特定的一组用例。我们仍然需要适用于更广泛用例的计算框架。

Apache Spark旨在成为一个快速，通用，易用的计算平台。它扩展了MapReduce模型并将其带到了另一个层次。速度来自内存计算。在内存中运行的应用程序允许更快的处理和响应。

## APACHE SPARK基础

[Apache Spark](https://zh.hortonworks.com/hadoop/spark/)是一种快速的内存数据处理引擎，在[Scala](https://spark.apache.org/docs/1.6.1/api/scala/index.html#package)，[Java](https://spark.apache.org/docs/1.6.1/api/java/index.html)，[Python](https://spark.apache.org/docs/1.6.1/api/python/index.html)和[R](https://spark.apache.org/docs/1.6.1/api/R/index.html)中具有优雅且富有表现力的开发[API](https://spark.apache.org/docs/1.6.1/api/R/index.html)，允许数据工作者有效地执行需要快速迭代访问数据集的机器学习算法。

在Apache Hadoop YARN上运行的Spark支持与企业中的Hadoop和其他支持YARN的工作负载进行深度集成。

您可以运行批处理应用程序，例如MapReduce类型作业或相互构建的迭代算法。您还可以运行交互式查询并使用您的应用程序处理流数据。Spark还提供了许多库，您可以轻松地使用这些库来扩展基本的Spark功能，例如机器学习算法，SQL，流和图形处理。Spark在Hadoop集群（如Hadoop YARN或Apache Mesos）上运行，甚至在具有自己的调度程序的独立模式下运行。Sandbox包括Spark 1.6和Spark 2.0。

![Lab4_1](pictures/83)

让我们开始吧！

## 使用AMBARI配置SPARK服务

1.登录Ambari仪表板`maria_dev`。在服务列的左下角，检查Spark2和Zeppelin Notebook是否正在运行。

2.使用浏览器URL打开Zeppelin界面：

```
http://sandbox-hdp.hortonworks.com:9995/
```

您应该看到Zeppelin欢迎页面：

![zeppelin_welcome_page](pictures/84)



或者，如果您想了解如何访问Spark shell上运行Spark代码参见[附录A](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/#run-spark-in-shell)。

3.创建Zeppelin笔记本

单击左上角的Notebook选项卡，然后选择**Create new note**。命名为`Compute Riskfactor with Spark`

![create_new_notebook_hello_hdp_lab4](pictures/85)



![notebook_name_hello_hdp_lab4](pictures/86)



## 创建一个HiveContext

为了改进Hive集成，已为Spark添加了[ORC文件](https://zh.hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/)支持。这允许Spark读取存储在ORC文件中的数据。Spark可以利用ORC文件更高效的列式存储和谓词下推功能，实现更快的内存处理。HiveContext是Spark SQL执行引擎的一个实例，它与存储在Hive中的数据集成在一起。更基本的SQLContext提供了不依赖于Hive的Spark SQL支持的子集。它从类路径上的hive-site.xml读取Hive的配置。

### 导入SQL库：

如果您已经浏览了Pig部分，则必须删除该表，`riskfactor`以便可以使用Spark再次填充它。将以下代码复制并粘贴到Zeppelin笔记本中，然后单击play 按钮。或者，按下以运行代码。`shift+enter`

```
%jdbc(hive) show tables
```

如果你看到一个名为的表`riskfactor`，删除它：

```
%jdbc(hive) drop table riskfactor
```

要确认表已被删除，让我们再次显示表：

```
%jdbc(hive) show tables
```

![drop_table_lab4](pictures/87)

### 实例化SPARKSESSION

```
%spark2
val hiveContext = new org.apache.spark.sql.SparkSession.Builder().getOrCreate()
```

## 从HIVE CONTEXT创建RDD

**什么是RDD？**

Spark的主要核心抽象称为弹性分布式数据集或RDD。它是跨集群并行化的分布式元素集合。换句话说，RDD是对象的不可变集合，其被分区并分布在YARN集群的多个物理节点上并且可以并行操作。

创建RDD有三种方法：

1. 并行化现有集合。这意味着数据已经驻留在Spark中，现在可以并行操作。
2. 通过引用数据集创建RDD。此数据集可以来自Hadoop支持的任何存储源，如HDFS，Cassandra，HBase等。
3. 通过转换现有RDD来创建RDD以创建新RDD。

我们将在本教程中使用后两种方法。

**RDD转换和操作**

通常，RDD通过从共享文件系统，HDFS，HBase或在YARN群集上提供Hadoop InputFormat的任何数据源加载数据来实例化。

实例化RDD后，您可以应用[一系列操作](https://spark.apache.org/docs/1.2.0/programming-guide.html#rdd-operations)。所有操作都属于以下两种类型之一：[转换](https://spark.apache.org/docs/1.2.0/programming-guide.html#transformations)或[操作](https://spark.apache.org/docs/1.2.0/programming-guide.html#actions)。



- 顾名思义，**转换**操作从现有RDD创建新数据集，并构建处理DAG，然后可以将其应用于跨YARN群集的分区数据集。转换不返回值。实际上，在定义这些转换语句期间没有确认任何内容。Spark只会创建这些直接非循环图或DAG，它们只会在运行时进行确认。我们称这种*懒惰的*确认。
- 一个**动作**操作，而另一方面，执行DAG并返回一个值。

### 查看HIVE WAREHOUSE中的表列表

使用简单的show命令查看Hive仓库中的表列表。

```scala
%spark2
hiveContext.sql("show tables").show()
```



![view_list_tables_hive_hello_hdp_lab4](pictures/88)

您会注意到我们之前在教程中创建的`geolocation`表和`drivermileage`表已经在**Hive Metastore中**列出，可以直接查询。

### 查询表以构建SPARK RDD

我们将执行一个简单的选择查询来从spark表变量获取数据`geolocation`和`drivermileage`表。以这种方式将数据导入Spark还允许将表模式复制到RDD。

```
%spark2
val geolocation_temp1 = hiveContext.sql("select * from geolocation")
```

![query_tables_build_spark_rdd_hello_hdp_lab4](pictures/89)

```
%spark2
val drivermileage_temp1 = hiveContext.sql("select * from drivermileage")
```

![drivermileage_spark_rdd_hello_hdp_lab4](pictures/90)



## 查询表

### 注册临时表

现在让我们注册临时表并使用SQL语法来查询该表。

```scala
%spark2
geolocation_temp1.createOrReplaceTempView("geolocation_temp1")
drivermileage_temp1.createOrReplaceTempView("drivermileage_temp1")
hiveContext.sql("show tables").show()
```

![name_rdd_hello_hdp_lab4](pictures/91)

接下来，我们将执行迭代和过滤操作。首先，我们需要过滤具有与之关联的非正常事件( non-normal events )的驱动程序，然后计算每个司机的非正常事件的数量。

```scala
%spark2
val geolocation_temp2 = hiveContext.sql("SELECT driverid, count(driverid) occurance from geolocation_temp1 where event!='normal' group by driverid")
```

![filter_drivers_nonnormal_events_hello_hdp_lab4](pictures/92)

- 如前所述，关于RDD转换，select操作是RDD转换，因此不返回任何内容。
- 结果表将计算与每个司机关联的非正常事件总数。将此筛选表注册为临时表，以便可以对其应用后续SQL查询。

```scala
%spark2
geolocation_temp2.createOrReplaceTempView("geolocation_temp2")
hiveContext.sql("show tables").show()
```

![register_filtered_table_hello_hdp_lab4](pictures/93)

- 您可以通过在RDD上执行操作操作来查看结果。

```scala
%spark2
geolocation_temp2.show(10)
```

![view_results_op_on_rdd_hello_hdp_lab4](pictures/94)

### 执行连接操作

在本节中，我们将执行连接操作geolocation_temp2表，其中包含司机的详细信息以及各自非正常事件的计数。drivermileage_temp1表格包含每位车手的总里程数。

- We will join two tables on common column, which in our case is `driverid`.
- 我们将以共同列`driverid`为标准合并两张表, 

```scala
%spark2
val joined = hiveContext.sql("select a.driverid,a.occurance,b.totmiles from geolocation_temp2 a,drivermileage_temp1 b where a.driverid=b.driverid")
```

![join_op_column_hello_hdp_lab4](pictures/95)



- 结果数据集将为特定驾驶员提供总里程和非正常事件总数。将此筛选表注册为临时表，以便可以对其应用后续SQL查询。

```scala
%spark2
joined.createOrReplaceTempView("joined")
hiveContext.sql("show tables").show()
```

![register_joined_table_hello_hdp_lab4](pictures/96)

- 您可以通过在RDD上执行操作操作来查看结果。

```scala
%spark2
joined.show(10)
```

![show_results_joined_table_hello_hdp_lab4](pictures/97)



### 计算驾驶员风险因素

在本节中，我们将驾驶员风险因素与每个驾驶员相关联。驾驶员风险因子将通过除以非正常事件发生的总行驶里程来计算。

```scala
%spark2
val risk_factor_spark = hiveContext.sql("select driverid, occurance, totmiles, totmiles/occurance riskfactor from joined")
```

![calculate_riskfactor_hello_hdp_lab4](pictures/98)



- 结果数据集将为我们提供总里程和非正常事件总数以及特定驾驶员的风险。将此筛选表注册为临时表，以便可以对其应用后续SQL查询。

```scala
%spark2
risk_factor_spark.createOrReplaceTempView("risk_factor_spark")
hiveContext.sql("show tables").show()
```

- 查看结果

```scala
%spark2
risk_factor_spark.show(10)
```

![view_results_filtertable_hello_hdp_lab4](pictures/99)

## 将数据作为ORC加载并保存到HIVE中

在本节中，我们使用Spark以智能ORC（优化行列式）格式存储数据。ORC是一种自描述的类型感知列文件格式，专为Hadoop工作负载而设计。它针对大型流式读取进行了优化，并具有快速查找所需行的集成支持。以列式格式存储数据使读者只能读取，解压缩和处理当前查询所需的值。因为ORC文件是类型感知的，所以编写器为类型选择最合适的编码，并在文件持久化时构建内部索引。

谓词下推使用这些索引来确定需要为特定查询读取文件中的哪些条数据，并且行索引可以将搜索范围缩小到特定的10,000行集。ORC支持Hive中的完整类型集，包括复杂类型：structs, lists, maps, and unions

### 创建ORC表

创建一个表并将其存储为ORC。在下面的SQL语句末尾指定为*orc*可确保Hive表以ORC格式存储。

```scala
%spark2
hiveContext.sql("create table finalresults( driverid String, occurance bigint, totmiles bigint, riskfactor double) stored as orc").toDF()
hiveContext.sql("show tables").show()
```

注意：toDF() 使用`driverid String, occurance bigint`,等列创建DataFrame

![create_orc_table_hello_hdp_lab4](pictures/101)

### 将数据转换为ORC表

在我们将数据加载到上面创建的Hive表之前，我们必须将我们的数据文件转换为ORC格式。

```scala
%spark2
risk_factor_spark.write.format("orc").save("risk_factor_spark")
```

![convert_orc_table_hello_hdp_lab4](pictures/102)

### 使用LOAD DATA命令将数据加载到HIVE表中

```scala
%spark2
hiveContext.sql("load data inpath 'risk_factor_spark' into table finalresults")
```

![load_data_to_finalresults_hello_hdp_lab4](pictures/103)



### 使用CTAS创建最终表RISKFACTOR

```scala
%spark
hiveContext.sql("create table riskfactor as select * from finalresults").toDF()
```

![create_table_riskfactor_spark](pictures/104)

### 验证HIVE中数据是否已成功填充HIVE表

执行select查询以验证表是否已成功存储。您可以转到Ambari Hive用户视图以检查您创建的Hive表是否已填充其中的数据。

![riskfactor_table_populated](pictures/105)



## 完整的SPARK代码回顾

**实例化SparkSession**

```scala
%spark2
val hiveContext = new org.apache.spark.sql.SparkSession.Builder().getOrCreate()
```

**显示默认Hive数据库中的表**

```scala
hiveContext.sql("show tables").show()
```

**从表中选择所有行和列，将Hive脚本存储到变量中并将变量注册为RDD**

```scala
val geolocation_temp1 = hiveContext.sql("select * from geolocation")

val drivermileage_temp1 = hiveContext.sql("select * from drivermileage")

geolocation_temp1.createOrReplaceTempView("geolocation_temp1")
drivermileage_temp1.createOrReplaceTempView("drivermileage_temp1")

val geolocation_temp2 = hiveContext.sql("SELECT driverid, count(driverid) occurance from geolocation_temp1 where event!='normal' group by driverid")

geolocation_temp2.createOrReplaceTempView("geolocation_temp2")
```

**从geolocation_temp2加载前10行，这是来自drivermileage表的数据**

```scala
geolocation_temp2.show(10)
```

**创建连接以通过相同的驱动程序连接2个表并将注册连接为RDD**

```scala
val joined = hiveContext.sql("select a.driverid,a.occurance,b.totmiles from geolocation_temp2 a,drivermileage_temp1 b where a.driverid=b.driverid")

joined.createOrReplaceTempView("joined")
```

**在连接中加载前10行和列**

```
joined.show(10)
```



**初始化risk_factor_spark并注册为RDD**

```scala
val risk_factor_spark = hiveContext.sql("select driverid, occurance, totmiles, totmiles/occurance riskfactor from joined")

risk_factor_spark.createOrReplaceTempView("risk_factor_spark")
```

**打印risk_factor_spark表中的前10行**

```
risk_factor_spark.show(10)
```



**在Hive中创建表final结果，将其保存为ORC，将数据加载到其中，然后使用CTAS创建名为riskfactor的最终表**

```scala
hiveContext.sql("create table finalresults( driverid String, occurance bigint, totmiles bigint, riskfactor double) stored as orc").toDF()

risk_factor_spark.write.format("orc").save("risk_factor_spark")

hiveContext.sql("load data inpath 'risk_factor_spark' into table finalresults")

hiveContext.sql("create table riskfactor as select * from finalresults").toDF()
```

## 总结

恭喜！让我们总结一下我们获得的Spark编码技巧和知识，用以计算与每个驾驶员相关的风险因素。Apache Spark因其**内存数据处理引擎**而具有高效的计算能力。我们通过创建**Hive Context**来学习如何将Hive与Spark集成。我们使用来自Hive的现有数据来创建**RDD**。我们学会了执行**RDD转换和操作**以从现有**RDD**创建新数据集。这些新数据集包括过滤，处理和处理的数据。在我们计算**风险因素后**，我们学习了如何将数据加载并存储到Hive中作为**ORC**。

## 进一步阅读

要了解有关Spark的更多信息，请查看以下资源：

- [Spark教程](https://zh.hortonworks.com/tutorials/?filters=apache-spark)
- [Apache Spark](https://zh.hortonworks.com/hadoop/spark/)
- [Apache Spark欢迎](http://spark.apache.org/)
- [Spark编程指南](http://spark.apache.org/docs/latest/programming-guide.html#passing-functions-to-spark)
- [学习Spark](http://www.amazon.com/Learning-Spark-Lightning-Fast-Data-Analysis/dp/1449358624/ref=sr_1_1?ie=UTF8&qid=1456010684&sr=8-1&keywords=apache+spark)
- [使用Spark进行高级分析](http://www.amazon.com/Advanced-Analytics-Spark-Patterns-Learning/dp/1491912766/ref=pd_bxgy_14_img_2?ie=UTF8&refRID=19EGG68CJ0NTNE9RQ2VX)



## 附录A：在SPARK INTERACTIVE SHELL中运行SPARK

1.使用[内置的SSH Web客户端](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#shell-web-client-method)（也称为shell-in-a-box），使用**maria_dev** / **maria_dev**登录

2.输入以下命令输入Spark交互式shell：



- `spark-shell`

This will load the default Spark Scala API. Issue the command `:help` for help and `:quit` to exit.

![spark_shell_welcome_page](pictures/106)

执行命令：

- `val hiveContext = new org.apache.spark.sql.SparkSession.Builder().getOrCreate()`
- `hiveContext.sql("show tables").show()`
- `val geolocation_temp1 = hiveContext.sql("select * from geolocation")`
- `val drivermileage_temp1 = hiveContext.sql("select * from drivermileage")`
- `geolocation_temp1.createOrReplaceTempView("geolocation_temp1")`
- `drivermileage_temp1.createOrReplaceTempView("drivermileage_temp1")`
- `hiveContext.sql("show tables").show()`
- `val geolocation_temp2 = hiveContext.sql("SELECT driverid, count(driverid) occurance from geolocation_temp1 where event!='normal' group by driverid")`
- `geolocation_temp2.createOrReplaceTempView("geolocation_temp2")`
- `hiveContext.sql("show tables").show()`
- `geolocation_temp2.show(10)`
- `val joined = hiveContext.sql("select a.driverid,a.occurance,b.totmiles from geolocation_temp2 a,drivermileage_temp1 b where a.driverid=b.driverid")`
- `joined.createOrReplaceTempView("joined")`
- `hiveContext.sql("show tables").show()`
- `joined.show(10)`
- `val risk_factor_spark = hiveContext.sql("select driverid, occurance, totmiles, totmiles/occurance riskfactor from joined")`
- `risk_factor_spark.createOrReplaceTempView("risk_factor_spark")`
- `hiveContext.sql("show tables").show()`
- `risk_factor_spark.show(10)`
- `hiveContext.sql("DROP TABLE IF EXISTS finalresults")`
- `hiveContext.sql("create table finalresults( driverid String, occurance bigint, totmiles bigint, riskfactor double) stored as orc").toDF()`
- `hiveContext.sql("show tables").show()`
- `risk_factor_spark.write.format("orc").mode("overwrite").save("risk_factor_spark")`
- `hiveContext.sql("load data inpath 'risk_factor_spark' into table finalresults")`
- `hiveContext.sql("DROP TABLE IF EXISTS riskfactor")`
- `hiveContext.sql("create table riskfactor as select * from finalresults").toDF()`
- `val riskfactor_temp = hiveContext.sql("SELECT * from riskfactor")`
- `riskfactor_temp.show(10)`









































































































































































