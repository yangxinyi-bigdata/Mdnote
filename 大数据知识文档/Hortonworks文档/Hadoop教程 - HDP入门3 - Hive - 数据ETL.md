# HIVE - 数据ETL

名词注释: DAG: 有向无环图

## 介绍

在本节中，您将了解Apache Hive。在前面的部分中，我们介绍了如何将数据加载到HDFS中。所以现在你已经将geolocation and trucks文件存储在HDFS中作为csv文件。为了在Hive中使用这些数据，我们将指导您如何创建表以及如何将数据移动到可以查询的Hive仓库中。我们将使用Hive用户视图中的SQL查询分析此数据并将其存储为ORC。当您将Tez指定为Hive的执行引擎时，我们还将介绍Apache Tez以及如何创建DAG。让我们开始…

## 先决条件

本教程是一系列教程的一部分，旨在帮助您使用Hortonworks沙箱开始使用HDP。在继续本教程之前，请确保完成先决条件。

- 已下载并已安装[Hortonworks Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- [学习HDP沙箱的绳索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- [传感器数据加载到HDFS中](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/2/#step-2---load-the-sensor-data-into-hdfs-)

## 大纲

- [Apache Hive基础知识](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#hive-basics)
- [第1步：熟悉Ambari Hive View](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#use-ambari-hive-user-views)
- [第2步：创建Hive表](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#define-a-hive-table)
- [第3步：在Ambari仪表板上探索Hive设置](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#explore-hive-settings)
- [第4步：分析trucks数据](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#analyze-truck-data)
- [概要](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#summary-lab2)
- [进一步阅读](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/#further-reading)

## APACHE HIVE基础知识

Apache Hive提供SQL接口来查询存储在与Hadoop集成的各种数据库和文件系统中的数据。Hive使熟悉SQL的分析师能够对大量数据运行查询。Hive有三个主要功能：数据汇总，查询和分析。Hive提供的工具可以轻松进行数据提取，转换和加载（ETL）。

## 第1步：熟悉AMBARI HIVE VIEW 2.0

Apache Hive提供HDFS中数据的关系视图。Hive可以用Hive管理的表格格式表示数据，也可以只存储在HDFS中，而不管存储数据的文件格式如何.Hive可以查询RCFile格式，文本文件，ORC，JSON，parquet，sequence 文件和其他许多文件中的数据。表格视图中的格式。通过使用SQL，您可以将数据视为表格，并像在RDBMS中一样创建查询。

为了便于与Hive交互，我们在Hortonworks Sandbox中使用了一个名为Ambari Hive View的工具。[Ambari Hive View 2.0](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-views/content/ch_using_hive_view.html)为[Hive](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-views/content/ch_using_hive_view.html)提供了一个交互式界面。我们可以创建，编辑，保存和运行查询，并让Hive使用一系列MapReduce作业或Tez作业为我们评估它们。

现在让我们打开Ambari Hive View 2.0并了解环境。转到Ambari用户视图图标并选择Hive View 2.0：

![selector_views_concepts](pictures/36)

Ambari Hive View如下所示：

![ambari_hive_user_view_concepts](pictures/37)



现在让我们仔细看看Hive View中的SQL编辑功能：

有6个标签可与Hive View 2.0交互：

1. **QUERY**：这是上面显示的界面和编写，编辑和执行新SQL语句的主界面。

2. **JOBS**：这使您可以查看过去和当前正在运行的查询。它还允许您查看您有权查看的所有SQL查询。例如，如果您是运营商且分析师需要查询帮助，则Hadoop运营商可以使用“历史记录”功能查看从报告工具发送的查询。

3. **TABLES**：提供一个集中的地方查看，创建，删除和管理表。

4. **SAVED QUERIES**：显示当前用户保存的查询。单击查询右侧的齿轮图标以在工作表中打开已保存的查询以进行编辑或执行。您还可以从保存的列表中删除已保存的查询。

5. **UDF**：可以通过指向HDFS上的JAR文件并指示包含UDF定义的Java类路径，将用户定义的函数（UDF）添加到查询中。在此处添加UDF后，查询编辑器中将出现一个“插入UDF”按钮，您可以将UDF添加到查询中。

6. **设置**：允许您修改将影响在Hive View中执行的查询的设置。

花几分钟时间探索各种Hive View子功能。

### 修改HIVEVIEW中的HIVE设置

在极少数情况下，您可能需要修改Hive设置。虽然您可以选择通过Ambari修改设置，但这是一种快速而简单的方法，无需重新启动Hive服务即可进行更改。在此示例中，我们将配置hive执行引擎以使用**tez**（这是默认值）。您可能想尝试map reduce（**mr**） - 执行查询时是否看到了差异？

1. 单击设置选项卡，在上面的界面中称为数字6
2. 单击**+添加新**
3. 单击KEY下拉菜单并选择 `hive.execution.engine`
4. 将值设置为 `tez`

## 第2步：创建HIVE表

现在您已熟悉Hive View，让我们为地理位置和卡车数据创建和加载表格。在本节中，我们将学习如何使用Ambari Hive View创建两个表：地理定位和卡车使用Hive View Upload Table选项卡。

### 创建和加载trucks表

从Hive View 2.0开始：

1. 选择**NEW TABLE**
2. 选择**UPLOAD TABLE**

![upload_table](pictures/38)



填写表格如下：

- 选择复选框：**是第一行标题**
- 选择**从HDFS上传**
- 设置**输入HDFS路径**，以`/user/maria_dev/data/trucks.csv`
- 单击**预览**Preview

![upload_table_hdfs_path_lab2](pictures/39)

你应该看到一个类似的屏幕：

> 注意：第一行包含列的名称。

![click_gear_button_lab2](pictures/40)

单击“ **创建”**按钮以完成表创建。

### 创建并加载地理位置表

使用该文件重复上述步骤以创建和加载地理位置表。`geolocation.csv`



### 在后台的工作过程

在回顾上传table过程中后台发生的事情之前，让我们先了解一下Hive文件格式。

[Apache ORC](https://orc.apache.org/)是Hadoop工作负载的快速柱状存储文件格式。

The Optimized Row Columnar（[Apache的ORC项目](https://zh.hortonworks.com/blog/apache-orc-launches-as-a-top-level-project/)）文件格式提供了存储Hive数据的一种高度有效的方法。它旨在克服其他Hive文件格式的限制。当Hive读取，写入和处理数据时，使用ORC文件可以提高性能。

要使用ORC文件格式创建表，请使用**STORED AS ORC**选项。例如：

```
CREATE TABLE <表名> ... 存储的AS ORC ... 
```

注意：有关这些子句的详细信息，请参阅[Apache Hive语言手册](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)。

以下是Upload表创建过程的直观表示：

1. 使用ORC文件格式（即地理位置）创建目标表
2. 使用TEXTFILE文件格式创建临时表以存储CSV文件中的数据
3. 数据从临时表复制到目标（ORC）表
4. 最后，临时表被删除

![create_tables_architecture](pictures/41)

您可以通过选择“ **JOBS”**选项卡并查看最近的四个作业来查看发出的SQL语句，这是使用“上**载表”**的结果。

![job_history_lab2](pictures/42)

### 验证新表存在

要验证表是否已成功定义：

1. 单击`TABLES`选项卡。
2. 单击资源管理器中的**刷新**图标`TABLES`。
3. 选择要验证的表。将显示列的定义。

### 来自trucks table的样本数据

单击`QUERY`选项卡，在查询编辑器中键入以下查询，然后单击`Execute`：

`select * from trucks limit10;`

结果应类似于：

![result_data_trucks](pictures/43)



**探索表的一些其他命令：**

- `show tables;` - 通过从HCatalogdescribe中存储的元数据中查找表列表，列出在数据库中创建的表
- `describe {table_name};` - 提供特定表的列

```
   describe geolocation;
```

- `show create table {table_name};` - 提供DDL以重新创建表

```
   show create table geolocation;
```

- `describe formatted {table_name};` - 探索有关该表的其他元数据。例如，您可以验证地理位置是否为ORC表，执行以下查询：

```
   describe formatted geolocation;
```



默认情况下，在Hive中创建表时，会在HDFS 的文件夹中创建具有相同名称的目录。使用Ambari文件视图，导航到/ apps / hive / warehouse文件夹。你应该看到一个和目录：`/apps/hive/warehouse``geolocation``trucks`

注意：Hive表的定义及其关联的元数据（即存储数据的目录，文件格式，Hive属性的设置等）存储在Hive Metastore中，它在Sandbox上是一个MySQL数据库。

### 重命名Rename/查询Query/编辑Editor 工作表

双击工作表选项卡将标签重命名为“sample truck data”。现在单击`Save`按钮保存此工作表。

![save_truck_sample_data_lab2](pictures/44)



### Beeline – Command Shell

尝试使用命令行界面运行--Beeline. Beeline使用JDBC连接来连接到HiveServer2. 

使用[内置的SSH Web客户端](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#shell-web-client-method)（也称为shell-in-a-box）：

1. 使用**maria_dev** / **maria_dev**登录

2. 连接到beeline 

   ```
   beeline -u jdbc:hive2://localhost:10000 -n maria_dev
   ```



3. 输入Beeline命令，例如：

```sql
!help
!tables
!describe trucks
select count(*) from trucks;
```

4. Exit the Beeline shell:

   ```
   !quit
   ```



从shell运行配置单元查询后，您注意到了什么表现？

- 使用shell的查询运行得更快，因为hive在hadoop中运行查询目录，而在Ambari Hive View中，查询必须在其提交给hadoop之前被rest server接受。
- 您可以[从Hive Wiki](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell)获得有关[Beeline的](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell)更多信息。
- Beeline 基于[SQLLine](http://sqlline.sourceforge.net/)。

## 第3步：在AMBARI仪表板上探索HIVE设置

### 在新标签中打开AMBARI仪表板

单击仪表板选项卡以开始浏览Ambari仪表板。

![ambari_dashboard_lab2](pictures/45)

### 熟悉HIVE设置

转到**Hive页面，**然后选择**Configs选项卡，**然后单击**Settings选项卡**：

![ambari_dashboard_explanation](pictures/46)

点击Hive页面后，您会看到类似于上面的页面：

1. Hive页面
2. Hive **Configs**选项卡
3. Hive **设置**选项卡
4. 版本配置**历史**

向下滚动到**优化设置**：

![hive_optimization_lab2](pictures/47)

在上面的截图中我们可以看到：

1. **Tez**被设置为优化引擎
2. **基于Cost的优化程序**（CBO）已打开

这显示了**HDP Ambari智能配置**，简化了设置配置

- Hadoop由一**组XML文件**配置。
- 在早期版本的Hadoop中，运营商需要进行**XML编辑**才能**更改设置**。没有默认版本控制。
- 早期的Ambari界面通过显示带有各种设置**对话框**的设置页面并允许您编辑它们，**更容易更改值**。但是，您需要了解需要进入该领域的内容并了解值的范围。
- 现在使用智能配置，您可以**切换二进制功能**并使用带有范围的设置的滑块。

默认情况下，密钥配置显示在第一页上。如果您要查找的设置不在此页面上，您可以在“ **高级”**选项卡中找到其他设置：

![hive_vectorization_lab2](pictures/48)



例如，如果我们想要**提高SQL性能**，我们可以使用新的**Hive矢量化功能**。可以通过以下步骤找到并启用这些设置：

1. 单击“ **高级”**选项卡，然后滚动以查找**属性**
2. 或者，开始在属性中键入属性搜索字段，然后这将过滤您滚动的设置。

从上面的绿色圆圈可以看出，已经打开了。`Enable Vectorization and Map Vectorization`

一些**关键资源**可以**了解有关矢量化的更多信息**以及**Hive调优中的**一些**关键设置：**

- 关于[矢量化查询执行的](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution) Apache Hive文档
- [HDP Docs矢量化文档](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.9.0/bk_dataintegration/content/ch_using-hive-1a.html)
- [Hive博客](https://zh.hortonworks.com/blog/category/hive/)
- [5种方法可以让您的Hive查询运行得更快](https://zh.hortonworks.com/blog/5-ways-make-hive-queries-run-faster/)
- [使用Tez作为快速查询引擎评估Hive](https://zh.hortonworks.com/blog/evaluating-hive-with-tez-as-a-fast-query-engine/)



## 第4步：分析trucks数据

接下来，我们将使用Hive，Pig和Zeppelin来分析地理位置和卡车表中的派生数据。业务目标是更好地了解公司因司机疲劳，过度使用的卡车以及各种卡车运输事件对风险的影响而面临的风险。为了实现这一点，我们将对源数据应用一系列转换，主要是通过SQL，并使用Pig或Spark来计算风险。在数据可视化的最后一个实验中，我们将使用Zeppelin 来**产生一系列的图表，以更好地了解风险**。

![Lab2_211](pictures/49)



让我们开始第一次转换。我们想要**计算每辆卡车的每加仑能行驶的英里数**。我们将从trucks数据表开始*。我们需要*在每辆卡车的基础上总结所有miles and gas columns*。Hive有一系列可用于重新格式化表的函数。关键字[LATERAL VIEW](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView)是我们调用事物的方式。堆栈功能允许我们*重组数据转换成3列*标记rdate，gas和mile（例如：“june13”，june13_miles，june13_gas）组成的最大的54行。我们从原始表中选择truckid，driverid，rdate，miles，gas，并为mpg（英里/气体）添加计算列。然后我们将**计算平均里程数**.

### 从现有的货运数据创建表trucks_mileage 

使用Ambari Hive View 2.0，执行以下查询：

```sql
CREATE TABLE truck_mileage STORED AS ORC AS SELECT truckid, driverid, rdate, miles, gas, miles / gas mpg FROM trucks LATERAL VIEW stack(54, 'jun13',jun13_miles,jun13_gas,'may13',may13_miles,may13_gas,'apr13',apr13_miles,apr13_gas,'mar13',mar13_miles,mar13_gas,'feb13',feb13_miles,feb13_gas,'jan13',jan13_miles,jan13_gas,'dec12',dec12_miles,dec12_gas,'nov12',nov12_miles,nov12_gas,'oct12',oct12_miles,oct12_gas,'sep12',sep12_miles,sep12_gas,'aug12',aug12_miles,aug12_gas,'jul12',jul12_miles,jul12_gas,'jun12',jun12_miles,jun12_gas,'may12',may12_miles,may12_gas,'apr12',apr12_miles,apr12_gas,'mar12',mar12_miles,mar12_gas,'feb12',feb12_miles,feb12_gas,'jan12',jan12_miles,jan12_gas,'dec11',dec11_miles,dec11_gas,'nov11',nov11_miles,nov11_gas,'oct11',oct11_miles,oct11_gas,'sep11',sep11_miles,sep11_gas,'aug11',aug11_miles,aug11_gas,'jul11',jul11_miles,jul11_gas,'jun11',jun11_miles,jun11_gas,'may11',may11_miles,may11_gas,'apr11',apr11_miles,apr11_gas,'mar11',mar11_miles,mar11_gas,'feb11',feb11_miles,feb11_gas,'jan11',jan11_miles,jan11_gas,'dec10',dec10_miles,dec10_gas,'nov10',nov10_miles,nov10_gas,'oct10',oct10_miles,oct10_gas,'sep10',sep10_miles,sep10_gas,'aug10',aug10_miles,aug10_gas,'jul10',jul10_miles,jul10_gas,'jun10',jun10_miles,jun10_gas,'may10',may10_miles,may10_gas,'apr10',apr10_miles,apr10_gas,'mar10',mar10_miles,mar10_gas,'feb10',feb10_miles,feb10_gas,'jan10',jan10_miles,jan10_gas,'dec09',dec09_miles,dec09_gas,'nov09',nov09_miles,nov09_gas,'oct09',oct09_miles,oct09_gas,'sep09',sep09_miles,sep09_gas,'aug09',aug09_miles,aug09_gas,'jul09',jul09_miles,jul09_gas,'jun09',jun09_miles,jun09_gas,'may09',may09_miles,may09_gas,'apr09',apr09_miles,apr09_gas,'mar09',mar09_miles,mar09_gas,'feb09',feb09_miles,feb09_gas,'jan09',jan09_miles,jan09_gas ) dummyalias AS rdate, miles, gas;
```



![create_table_truckmileage](pictures/50)

### 探索trucks_mileage表中的数据样本

要查看脚本生成的数据，请在查询编辑器中执行以下查询：

```sql
select * from truck_mileage limit 100 ;   
```



您应该看到一张表格，*列出卡车和司机的每次旅行*：

![select_data_truck_mileage_lab2](pictures/51)

### 使用内容助手来构建查询

1.创建一个新的**SQL工作表**。

2.开始键入**SELECT SQL命令**，但只输入前两个字母：

```
SE
```

3.按**Ctrl +空格**键以查看以下内容辅助弹出对话框窗口：

![Lab2_24](pictures/52)

>  注意：通知内容帮助会显示一些以“SE”开头的选项。当您编写大量自定义查询代码时，这些快捷方式将非常适合。

4.在整个输入过程中使用**Ctrl +空格**键入以下查询，以便了解内容辅助可以执行的操作及其工作方式：

```
SELECT truckid, avg(mpg) avgmpg FROM truck_mileage GROUP BY truckid;
```

![Lab2_28](pictures/53)



5.单击“ **另存为**/Save as ”按钮将查询保存为“ average mpg ”：

![Lab2_26](pictures/54)



6.请注意，您的查询现在显示在“ **已保存的查询** ” 列表中，该列表是Hive用户视图顶部的选项卡之一。

7.执行“ **average mpg** ”查询并查看其结果。

### 探索HIVE查询编辑器的说明功能/Explain Features

让我们探索各种解释功能，以便更好地*理解查询的执行*：Visual Explain，Text Explain和Tez Explain。单击**Visual Explain**按钮：

![Lab2_27](pictures/55)

此可视化解释提供了查询执行计划的可视摘要。您可以通过单击每个计划阶段来查看更多详细信息。

![visual_explain_dag_lab2](pictures/56)



如果要在文本中查看说明结果，请选择`RESULTS`。你应该看到类似的东西：

![tez_job_result_lab2](pictures/57)



### 探索TEZ

点击**Ambari** Views的**TEZ View**。您可以看到与之前的Hive配置单元和Pig作业相关的*DAG详细信息*。

![Amberi_tez_wu_lb2](pictures/58)



选择第一个，`DAG ID`因为它表示执行的最后一个作业。

![tez_view_dashboard_lab](pictures/59)



顶部有七个标签，请花几分钟时间浏览各个标签。完成浏览后，单击“ **图形视图/Graphical View”**选项卡并使用光标将鼠标悬停在其中一个节点上，以获取有关该节点中处理的更多详细信息。

![tez_graphical_view_lab2](pictures/60)



单击**Vertex Swimlane**。此功能有助于TEZ作业的故障排除。正如您将在图像中看到的，有一个Map 1和Reducer 2的图表。这些图表是事件发生时的时间轴。将鼠标悬停在红线或蓝线上可查看事件工具提示。

基本术语：

- Bubble 代表一个事件
- Vertex 表示事件的实线，时间轴

对于map1，工具提示显示事件vertex 开始并且vertex 初始化同时发生：

![tez_vertex_swimlane_map1_lab2](pictures/61)



对于Reducer 2，工具提示显示事件vertex 开始并初始化执行时间的共享毫秒差异。

vertex 初始化

![tez_vertex_swimlane_reducer2_initial_lab2](pictures/62)



vertex 开始了

![tez_vertex_swimlane_reducer2_started_lab2](pictures/63)



当您查看已开始和完成的任务（红色粗线 Map 1）与图表中的（蓝色粗线 Reducer 2）相比时，您注意到了什么？

- `Map 1` starts and completes before `Reducer 2`.

### 从现有的trucks_mileage 数据创建表avg_mileage

通常将查询结果保存到表中，以便结果集变为持久性的。这称为[Create Table As Select](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-CreateTableAsSelect)（CTAS）。将以下DDL复制到查询编辑器中，然后单击“ **执行”**：

```sql
CREATE TABLE avg_mileage
STORED AS ORC
AS
SELECT truckid, avg(mpg) avgmpg
FROM truck_mileage
GROUP BY truckid;
```



![create_avg_mileage_table_lab2](pictures/64)

### 查看avg_mileage的示例数据

要查看上面CTAS生成的数据，请执行以下查询：

```sql
SELECT * FROM avg_mileage LIMIT 100 ;
```



表格`avg_mileage`列出了每辆卡车每加仑的平均里程数。

![load_sample_avg_mileage_lab2](pictures/65)



### 从现有的truck_mileage 数据创建表DriverMileage 

以下CTAS按驱动程序和里程总和对记录进行分组。将以下DDL复制到查询编辑器中，然后单击“ **执行”**：

```sql
CREATE TABLE DriverMileage
STORED AS ORC
AS
SELECT driverid, sum(miles) totmiles
FROM truck_mileage
GROUP BY driverid;
```



![driver_mileage_table_lab3](pictures/66)

### 查看DriverMileage的数据

要查看上面CTAS生成的数据，请执行以下查询：

```sql
SELECT * FROM drivermileage LIMIT 100 ;   
```

![select_data_drivermileage_lab2](pictures/67)



## 总结

恭喜！让我们总结一下学习了什么, 我们学会了处理，过滤, 操纵地理位置和卡车数据的一些Hive命令。 

我们可以使用`CREATE TABLE`和`UPLOAD TABLE`创建Hive表。

我们学习了如何将表的文件格式更改为ORC，这样hive在读取，写入和处理此数据时更有效。我们学会了使用`SELECT`statement 检索数据并创建一个新的过滤表（`CTAS`）。



## 进一步阅读

使用以下资源增强您的蜂巢基础：

- [Apache Hive](https://zh.hortonworks.com/hadoop/hive/)
- [Hive LLAP在Hadoop上启用了第二个SQL](https://zh.hortonworks.com/blog/llap-enables-sub-second-sql-hadoop/)
- [编程Hive](http://www.amazon.com/Programming-Hive-Edward-Capriolo/dp/1449319335/ref=sr_1_3?ie=UTF8&qid=1456009871&sr=8-3&keywords=apache+hive)
- [Hive语言手册](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)
- [HDP开发者：APACHE PIG和HIVE](https://zh.hortonworks.com/training/class/hadoop-2-data-analysis-pig-hive/)



















