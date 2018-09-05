## 介绍

在本教程中，我们将使用[Ambari](https://zh.hortonworks.com/hadoop/ambari/) HDFS文件视图来存储卡车司机统计数据文件。我们将实现[Hive](https://zh.hortonworks.com/hadoop/hive/)查询来分析，处理和过滤该数据。

## 先决条件

- 已下载并安装最新的[Hortonworks Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- [学习HDP沙箱的绳索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- 请大约一个小时完成本教程

## 大纲

- [Pig](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#hive)
- [Hive还是Pig？](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#hive-or-pig)
- [数据处理任务](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#our-data-processing-task)
- [第1步：下载数据](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#download-the-data)
- [第2步：上传数据文件](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#upload-the-data-files)
- [第3步：启动Hive View](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#start-the-hive-view)
- [总结](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#summary)
- [进一步阅读](https://zh.hortonworks.com/tutorial/how-to-process-data-with-apache-hive/#further-reading)

## Hive

Apache Hive是[Hortonworks Data Platform](https://zh.hortonworks.com/hdp/)（HDP）的一个组件。Hive为存储在HDP中的数据提供类似SQL的接口。在上一个教程中，我们使用了Pig，它是一种专注于数据流的脚本语言。Hive为Apache Hadoop提供了一个数据库查询接口。

## Hive还是Pig？

人们经常会问，为什么[Pig](https://zh.hortonworks.com/hadoop/pig/)和[Hive](https://zh.hortonworks.com/hadoop/hive/)似乎在做同样的事情。Hive因其类似SQL的查询语言经常被用作基于Apache Hadoop的数据仓库的接口。对于习惯使用SQL查询数据的用户来说，Hive被认为更友好，更熟悉。Pig通过其数据流优势适应了将数据引入Apache Hadoop并使用它进入查询形式的任务。关于这个问题的一个很好的说明是艾伦·盖茨在雅虎开发者博客上发布的一篇名为“ [Pig和Hive](http://yahoohadoop.tumblr.com/post/98256601751/pig-and-hive-at-yahoo) ”的雅虎博客。从技术角度来看，Pig和Hive都是功能齐全的，因此您可以在任一工具中执行任务。但是，您会发现一个工具或另一个工具将被必须使用Apache Hadoop的不同组首选。好消息是他们有另外一个选择, 就是让他们两个同时工作.

## 我们的数据处理任务

我们将在在Hive中执行与之前的教程中Pig执行过的相同的数据处理任务。

我们有几个卡车司机统计文件，我们会把它们传入到Hive中, 并用它们做一些简单的计算。我们将计算卡车司机驾驶一年的记录小时数和里程数。当我们记录了小时和英里的总和，我们将扩展脚本，通过连接两个不同的表将`driver id`字段转换为`the name of the drivers`的名称。



## 第1步：下载数据

从[这里](https://raw.githubusercontent.com/hortonworks/data-tutorials/master/tutorials/hdp/how-to-process-data-with-apache-hive/assets/driver_data.zip)下载驱动程序数据文件。
获得文件后，您需要将文件解压缩到一个目录中。我们将上传两个csv文件 - 和。`drivers.csv``timesheet.csv`

## 第2步：上传数据文件

我们首先从顶部的Off-canvas菜单中选择。HDFS文件视图允许我们查看Hortonworks数据平台（HDP）文件存储。这与本地文件系统是分开的。对于Hortonworks Sandbox，它将成为Hortonworks Sandbox VM中文件系统的一部分。`HDFS Filesview`

[![select_files_view](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/select_files_view.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/select_files_view.jpg)

导航到并单击按钮以选择要上载到Hortonworks Sandbox环境中的文件。`/user/maria_dev``Upload`

[![upload_button](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/upload_button-800x301.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/upload_button.jpg)

单击`browse`按钮以打开对话框。导航到您在本地磁盘上存储文件的位置，然后选择并单击。做同样的事情。完成后，您将看到目录中有两个新文件。`drivers.csv``drivers.csv``open``timesheet.csv`

[![uploaded_files](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/uploaded_files-800x325.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/uploaded_files.jpg)

## 第3步：启动HIVE VIEW

让我们点击顶部栏上的“视图”图标打开。Hive View 2.0为Hadoop的Hive数据仓库系统提供了用户界面。`Hive View 2.0`

[![select_hive_view](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/select_hive_view.jpg)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/select_hive_view.jpg)

### 3.1探索HIVE用户界面

下面是。查询可能跨越多行。在底部，有查询按钮，查询，带有名称的查询以及为另一个查询打开新的工作表窗口。`QueryEditor``Execute``Visual Explain``Save As`

[![hive_view_home_page](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/hive_view_home_page-800x473.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/hive_view_home_page.png)

#### Hive和Pig数据模型的差异

在我们开始之前，让我们来看看如何 `Pig and Hive`数据模型不同。在Pig的情况下，所有数据对象都存在并在脚本中进行操作。脚本完成后，除非您存储它们，否则将删除所有数据对象。在Hive的情况下，我们在Apache Hadoop数据存储上运行。您创建的任何查询，您创建的表，您复制的数据都会从查询到查询持续存在。您可以将Hive视为提供数据工作台，您可以在其中检查，修改和操作Apache Hadoop中的数据。因此，当我们执行数据处理任务时，我们将一次执行一个查询或行。成功执行一行后，您可以查看数据对象以验证上一次操作是否符合预期。与Pig相比，您的所有数据都是实时的，其中数据对象仅存在于脚本中，除非它们被复制到存储中。这种灵活性是Hive的强项。

### 3.2创建表TEMP_DRIVERS

我们要做的第一项任务是创建一个表来保存数据。我们将查询输入到。输入查询后，点击底部的按钮。`QueryEditor``Execute`

```
create table temp_drivers (col_value STRING);
```

[![create_temp_drivers](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/create_temp_drivers-800x446.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/create_temp_drivers.png)

> **提示：**按`CTRL`+ `Space`进行自动完成

查询不返回任何结果，因为此时我们刚刚创建了一个空表，并且我们没有复制任何数据。
查询执行完毕后，我们可以`Database`通过重新选择来刷新`Database`。我们将看到名为的新表`temp_drivers`。

[![database_explorer](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/database_explorer-800x442.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/database_explorer.png)

### 3.3创建查询以使用DRIVERS.CSV数据填充HIVE表TEMP_DRIVERS

下一行代码将数据文件加载到表中。`drivers.csv``temp_drivers`

```
LOAD DATA INPATH '/user/maria_dev/drivers.csv' OVERWRITE INTO TABLE temp_drivers;
```

[![load_data_temp_drivers](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/load_data_temp_drivers-800x450.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/load_data_temp_drivers.png)

执行后`LOAD DATA`我们可以看到表`temp_drivers`中填充了来自的数据。请注意，Hive 在此步骤中使用了数据文件。如果您查看，您将看到drivers.csv不再存在。`drivers.csv``drivers.csv``File Browser`

[![select_data_temp_drivers](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_temp_drivers-800x605.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_temp_drivers.png)

### 3.4创建表驱动程序

现在我们已经阅读了数据，我们可以开始使用它了。接下来我们要做的就是提取数据。首先，我们将输入一个查询来创建一个名为`drivers`保存数据的新表。该表将有六列驱动程序。`driverId, name, ssn, location, certified and the wage-plan`

```
CREATE TABLE drivers (driverId INT, name STRING, ssn BIGINT, location STRING, certified STRING, wageplan STRING);
```

[![create_table_drivers](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/create_table_drivers-800x363.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/create_table_drivers.png)

### 3.5创建查询以从TEMP_DRIVERS中提取数据并将其存储到驱动程序

然后我们提取我们想要的数据`temp_drivers`并将其复制到`drivers`。我们将用一种`regexp`模式来做到这一点。为此，我们将构建一个多行查询。六个regexp_extract调用将从表temp_drivers中提取字段。键入查询后，它将如下所示。请注意，正则表达式模式中没有空格。`driverId, name, ssn, location, certified and the wage-plan`

```
insert overwrite table drivers
SELECT
  regexp_extract(col_value, '^(?:([^,]*),?){1}', 1) driverId,
  regexp_extract(col_value, '^(?:([^,]*),?){2}', 1) name,
  regexp_extract(col_value, '^(?:([^,]*),?){3}', 1) ssn,
  regexp_extract(col_value, '^(?:([^,]*),?){4}', 1) location,
  regexp_extract(col_value, '^(?:([^,]*),?){5}', 1) certified,
  regexp_extract(col_value, '^(?:([^,]*),?){6}', 1) wageplan

from temp_drivers;
```

[![insert_into_drivers](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/insert_into_drivers-800x375.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/insert_into_drivers.png)

执行查询并查看`drivers`表。您应该看到看起来像这样的数据。

[![select_data_drivers](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_drivers-800x520.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_drivers.png)

### 3.6类似地创建TEMP_TIMESHEET和TIMESHEET表

同样，我们必须创建一个名为的表`temp_timesheet`，然后加载示例文件。逐个键入以下查询：`timesheet.csv`

```
CREATE TABLE temp_timesheet (col_value string);
LOAD DATA INPATH '/user/maria_dev/timesheet.csv' OVERWRITE INTO TABLE temp_timesheet;
```

你应该看到这样的数据：

[![select_data_temp_timesheet](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_temp_timesheet-800x533.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_temp_timesheet.png)

现在`timesheet`使用以下查询创建表：

```
CREATE TABLE timesheet (driverId INT, week INT, hours_logged INT , miles_logged INT);
```

使用与之前相同的方法将数据`timesheet`从`temp_timesheet`表中插入表中`regexp_extract`。

```
insert overwrite table timesheet
SELECT
  regexp_extract(col_value, '^(?:([^,]*),?){1}', 1) driverId,
  regexp_extract(col_value, '^(?:([^,]*),?){2}', 1) week,
  regexp_extract(col_value, '^(?:([^,]*),?){3}', 1) hours_logged,
  regexp_extract(col_value, '^(?:([^,]*),?){4}', 1) miles_logged

from temp_timesheet;
```

你应该看到这样的数据：

[![select_data_timesheet](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_timesheet-800x534.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/select_data_timesheet.png)

### 3.7创建查询以过滤数据（DRIVERID，HOURS_LOGGED，MILES_LOGGED）

现在我们有了我们想要的数据字段。下一步是`group`通过driverId获取数据，这样我们就可以找到`sum`一年内记录的小时数和里程数。此查询首先对所有记录进行分组`driverId`，然后选择具有该年度记录的小时数和英里数的总和的驱动程序。

```
SELECT driverId, sum(hours_logged), sum(miles_logged) FROM timesheet GROUP BY driverId;
```

查询结果如下所示：

[![group_data](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/group_data-800x566.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/group_data.png)

### 3.8创建查询以加入数据（DRIVERID，NAME，HOURS_LOGGED，MILES_LOGGED）

现在我们需要返回并获取所以我们知道驱动程序是谁。我们可以使用先前的查询并将其与记录连接以获得具有的最终表。`driverId(s)``drivers``driverId, name and the sum of hours and miles logged`

```
SELECT d.driverId, d.name, t.total_hours, t.total_miles from drivers d
JOIN (SELECT driverId, sum(hours_logged)total_hours, sum(miles_logged)total_miles FROM timesheet GROUP BY driverId ) t
ON (d.driverId = t.driverId);
```

结果数据如下：

[![join_data](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/join_data-800x631.png)](https://2xbbhjxc6wk3v21p62t8n4d4-wpengine.netdna-ssl.com/wp-content/uploads/2017/06/join_data.png)

所以现在我们有了结果。如前所述，我们一步一步地使用Hive解决了这个问题。在任何时候我们都可以自由地查看数据，决定我们需要做另一项任务并回来。在任何时候，数据都是实时的，我们可以访问。

## 概要

恭喜您完成本教程！我们刚学会了如何将数据上传到HDFS文件视图并创建配置单元来操纵数据。让我们回顾一下被用在本教程中的所有查询：**创建**，**加载**，**插入**，**选择**，**从**，**按组**，**加入**和**上**。通过这些查询，我们创建了一个表*temp_drivers*来存储数据。我们创建了另一个表*驱动程序*，因此我们可以使用我们之前创建的*temp_drivers*表中提取的数据覆盖该表。然后我们又用同样的*temp_timesheet*和*时间表*。最后，创建的查询来筛选数据为具有结果显示由每个驱动程序将记录小时，英里的总和。

## 进一步阅读

- [Apache Hive](https://zh.hortonworks.com/apache/hive/)
- [Hive 教程](https://zh.hortonworks.com/apache/hive/#tutorials)
- [Hive语言手册](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)