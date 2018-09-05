# Spark REPL中的DataFrame和Dataset示例

## 介绍

本教程将帮助您开始使用Apache Spark，并将涵盖：

- 如何使用Spark DataFrame和Dataset API
- 如何通过[Shell-in-a-Box](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#terminal-access)使用SparkSQL接口

## 先决条件

- [下载并安装最新的Hortonworks数据平台（HDP）沙箱](https://zh.hortonworks.com/downloads/#sandbox)
- [学习HDP沙箱的绳索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- 基本[Scala](http://www.dhgarrette.com/nlpclass/scala/basics.html)语法
- [Apache Zeppelin入门](https://zh.hortonworks.com/tutorial/getting-started-with-apache-zeppelin/)

## 大纲

- [概念](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#concepts)
- [环境设置](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#environment-setup)
- [DataFrame API示例](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#dataframe-api-example)
- [DataSet API示例](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#dataset-api-example)
- [结论](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#conclusion)
- [进一步阅读](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#further-reading)



## 概念

### SPARK SQL

Spark SQl是用于结构化数据处理的Spark模块。它具有为Spark提供有关数据结构和正在执行的计算的附加信息的接口。附加信息用于优化。

您可以使用Spark SQL执行的操作：

- 执行SQL查询
- 从现有Hive安装中读取数据

### 数据集和数据框架

数据集是一种接口，它提供RDD（强类型）和Spark SQL优化的好处。重要的是要注意，数据集可以从JVM对象构造，然后使用复杂的功能转换进行操作，但是，它们超出了这个快速指南。

DataFrame是组织为命名列的数据集。从概念上讲，它们相当于关系数据库中的表或R或Python中的DataFrame。可以从各种来源创建DataFrame，例如：

1.结构化数据文件

2.蜂巢中的表格

3.外部数据库

4.现有的RDD

数据集和DataFrame之间的主要区别在于数据集是强类型的。

详细了解[数据集和数据框架](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes)。

## 环境设置

### 下载数据集

在准备本教程时，您需要将两个文件people.txt和people.json下载到Sandbox的**tmp**文件夹中。下面的命令应该输入Shell-in-a-Box

1. 假设你以`root`用户身份开始：

```scala
cd /tmp
```

2. 复制并粘贴命令以下载people.txt：

```scala
#Download people.txt
wget https://raw.githubusercontent.com/hortonworks/data-tutorials/master/tutorials/hdp/dataFrame-and-dataset-examples-in-spark-repl/assets/people.txt
```

3. 复制并粘贴命令以下载people.json：

```scala
#Download people.json
wget https://raw.githubusercontent.com/hortonworks/data-tutorials/master/tutorials/hdp/dataFrame-and-dataset-examples-in-spark-repl/assets/people.json
```

### 将数据集上传到HDFS

1. 在将文件移动到HDFS之前，您需要在**hdfs**用户下登录，以便为root用户授予执行文件操作的权限：

```shell
＃以hdfs用户身份登录，为文件操作授予root权限
su hdfs
cd
```

2. 接下来，将people.txt和people.json文件上传到HDFS：

```shell
＃将文件从本地系统复制到HDF 
hdfs dfs -put /tmp/people.txt /tmp/people.txt
hdfs dfs -put /tmp/people.json /tmp/people.json
```

3. 通过复制以下命令验证两个文件都已复制到HDFS **/ tmp**文件夹：

```shell
＃验证文件是否已移至HDF 
hdfs dfs -ls /tmp
```

现在，我们准备开始这些例子了。

4.启动Spark Shell：

```shell
spark-shell
```

## DATAFRAME API示例

DataFrame API提供了更简单的数据访问，因为它在概念上看起来像一个表，许多Python / R / Pandas的开发人员都熟悉它。

在REPL提示符下，键入以下内容：`scala>`

```scala
val df = spark.read.json("/tmp/people.json")
```

使用，显示DataFrame的内容：`df.show`

```scala
df.show
```

您应该看到类似于的输出：

```scala
...
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+

scala>
```

### 其他DATAFRAME API示例

现在，让我们选择“name”和“age”列，并将“age”列增加1：

```scala
df.select(df("name"), df("age") + 1).show() 
```

这将产生类似于以下的输出：

```scala
...
+-------+---------+
|   name|(age + 1)|
+-------+---------+
|Michael|     null|
|   Andy|       31|
| Justin|       20|
+-------+---------+

scala>
```

要返回21岁以上的人，请使用filter（）函数：

```scala
df.filter(df("age") > 21).show()
```

这将产生类似于以下的输出：

```scala
...
+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+

scala>
```

接下来，要计算特定年龄的人数，请使用groupBy（）和count（）函数：

```scala
df.groupBy("age").count().show()
```

这将产生类似于以下的输出：

```scala
...
+----+-----+
| age|count|
+----+-----+
|  19|    1|
|null|    1|
|  30|    1|
+----+-----+

scala>
```

### 以编程方式指定架构

在Spark-shell中键入以下命令（一行一行）：

1. 导入必要的库

```scala
import org.apache.spark.sql._
import org.apache.spark.sql.Row
import org.apache.spark.sql.types._
import spark.implicits._
```

2. 创建和RDD

```scala
val peopleRDD = spark.sparkContext.textFile("/tmp/people.txt")
```

3. 将Schema编码为字符串

```scala
val schemaString = "name age"
```

4. 基于模式字符串生成模式

```scala
val fields = schemaString.split(" ").map(fieldName => StructField(fieldName, StringType, nullable = true))

val schema = StructType(fields)
```

5. 将RDD（人）的记录转换为行

```scala
val rowRDD = peopleRDD.map(_.split(",")).map(attributes => Row(attributes(0), attributes(1).trim))
```

6. 将架构应用于RDD

```scala
val peopleDF = spark.createDataFrame(rowRDD, schema)
```

6. 使用DataFrame创建临时视图

```scala
peopleDF.createOrReplaceTempView("people")
```

7. SQL可以在使用DataFrames创建的临时视图上运行

```scala
val results = spark.sql("SELECT name FROM people")
```

8. SQL查询的结果是DataFrames并支持所有正常的RDD操作。结果中的行的列可以通过字段索引或字段名称来访问

```scala
results.map(attributes => "Name: " + attributes(0)).show()
```

这将产生类似于以下的输出：

```scala
...
+-------------+
|        value|
+-------------+
|Name: Michael|
|   Name: Andy|
| Name: Justin|
+-------------+

scala>
```

## DATASET API示例

如果您在前面的部分中尚未这样做，请确保将人员数据集（people.txt和people.json）上传到HDFS：[Environment Setup](https://zh.hortonworks.com/tutorial/dataframe-and-dataset-examples-in-spark-repl/#environment-setup)并在上面以**编程方式指定架构的**步骤1中导入库：

最后，如果你还没有：

启动Spark Shell

```scala
spark-shell
```

Spark数据集API将最好的RDD和数据框结合在一起，用于直接在现有JVM类型上运行的类型安全和用户功能。

让我们尝试通过将*toDS（）*函数应用于数字序列来创建数据集的最简单示例。

在提示符下，复制并粘贴以下内容：`scala>`

```scala
val ds = Seq(1, 2, 3).toDS()

ds.show
```

您应该看到以下输出：

```scala
+-----+
|value|
+-----+
|    1|
|    2|
|    3|
+-----+
```

继续进行一个稍微有趣的例子，让我们准备一个*Person*类来保存我们的人员数据。我们将以两种方式使用它，将其直接应用于硬编码数据，然后应用于从json文件读取的数据。

要将*Person*类应用于硬编码数据类型：

```scala
case class Person(name: String, age: Long)

val ds = Seq(Person("Andy", 32)).toDS()
```

当你输入

```scala
ds.show
```

您应该看到*ds* Dataset 的以下输出

```scala
+----+---+
|name|age|
+----+---+
|Andy| 32|
+----+---+
```

最后，让我们将从*people.json*读取的数据*映射*到*Person*类。映射将按名称完成。

```scala
val path = "/tmp/people.json"
val people = spark.read.json(path).as[Person] // Creates a DataSet
```

要查看人员DataFrame类型的内容：

```scala
people.show
```

您应该看到类似于以下内容的输出：

```scala
...
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+
```

请注意，*age*列包含*空*值。在我们将人员DataFrame转换为数据集之前，让我们先过滤掉*null*值：

```scala
val pplFiltered = people.filter("age is not null")
```

现在我们可以映射到*Person*类并将我们的DataFrame转换为数据集。

```scala
val pplDS = pplFiltered.as[Person]
```

查看数据集类型的内容

```scala
pplDS.show
```

你应该看到以下内容：

```scala
+------+---+
|  name|age|
+------+---+
|  Andy| 30|
|Justin| 19|
+------+---+
```

退出类型：

```scala
:quit
```

# 概要

恭喜！您现在已经了解了有关Spark SQL，数据集和DataFrame在Spark中的角色的更多信息。此外，您还学习了如何通过Shell-in-a-Box使用SparkSQL接口并查看数据集的内容。

## 进一步阅读

接下来，[在Zeppelin Notebook中](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/)探索更高级的[SparkSQL命令](https://zh.hortonworks.com/tutorial/learning-spark-sql-with-zeppelin/)。




