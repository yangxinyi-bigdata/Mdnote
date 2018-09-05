## Pig – Risk Factor

## 介绍

在本教程中，您将了解[Apache Pig](https://zh.hortonworks.com/hadoop/pig/)。在实验的前面部分中，您学习了如何将数据加载到HDFS中，然后使用Hive对其进行操作。我们正在使用卡车传感器数据来更好地了解与每个驾驶员相关的风险。本节将教您**使用Apache Pig计算风险**。

## 先决条件

本教程是使用Hortonworks沙箱开始使用HDP的系列教程的一部分。在继续本教程之前，请确保完成先决条件。

- 已下载并已安装[Hortonworks Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- [学习HDP沙箱的线索](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
- [将传感器数据加载到HDFS中](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/2/)
- [Hive - 数据ETL](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/)



## 大纲

- [Pig基础知识](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/#pig-basics)
- [创建Pig脚本](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/#create-pig-script)
- [快速回顾](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/#quick-recap)
- [在Tez上执行Pig Script](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/#execute-pig-script-on-tez)
- [概要](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/#summary)
- [进一步阅读](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/#further-reading)

## Pig Basics

Pig是Apache Hadoop使用**的高级脚本语言**。Pig使数据工作者能够在不知道Java的情况下**编写复杂的数据转换**。Pig *简单的类似SQL的脚本语言*叫做Pig Latin，并且对那些已经熟悉脚本语言和SQL的开发人员很有吸引力。

Pig是完整的，因此您可以使用Pig在Apache Hadoop中完成所有必需的数据操作。通过Pig中的**用户定义函数**（UDF）工具，Pig可以调用许多语言中的代码，如*JRuby，Jython和Java*。您还可以使用其他语言嵌入Pig脚本。结果是您可以使用Pig作为组件来构建更大，更复杂的应用程序来解决实际业务问题。

Pig使用来自许多来源的数据，包括**结构化和非结构化数据**，并将结果存储到Hadoop数据文件系统中。

Pig脚本被转换为一系列**在Apache Hadoop集群**上运行的**MapReduce作业**。



### 从现有的trucks_mileage 数据创建风险因素表



接下来，您将使用Pig计算每个司机的风险因素。在我们运行Pig代码之前，该*表必须已经存在于Hive中*以满足*HCatStorer() class的*一个*要求*。Pig代码需要以下结构来表示名称`riskfactor`。在Hive View 2.0查询编辑器中执行以下DDL：

```sql
CREATE TABLE riskfactor (driverid string, events bigint, totmiles bigint, riskfactor float)
STORED AS ORC;
```

![riskfactor_lab3](pictures/68)



### 确认风险因素表已成功创建

验证`riskfactor`表是否已成功创建。它现在是空的，但您将使用Pig脚本填充它。您现在可以使用Pig计算风险因素了。让我们来看看Pig以及如何从Ambari中执行Pig脚本。

## 创建PIG脚本

在本教程的这个阶段，我们创建并运行Pig脚本。我们将使用Ambari Pig View。让我们开始吧…



### 登录AMBARI PIG用户视图

要进入Ambari Pig View，请单击右上角的Ambari Views图标并选择**Pig**：

![ambari_pig_view_concepts](pictures/69)

这将打开Ambari Pig用户界面界面。您的Pig View没有要显示的脚本，因此它将如下所示：

![Lab3_4](pictures/70)

左侧是脚本列表，右侧是用于编写脚本的组合框。一个**特殊的接口功能**是位于脚本文件名称下方的*Pig助手*。该Pig助手为我们提供了statements/语句，functions/函数，I / O语句，HCatLoader() 的模板和Python用户定义的函数。最底部是状态区域，将显示我们的脚本和日志文件的结果。

以下屏幕截图显示并描述了Pig View的各种组件和功能：

1. 快速链接以查看现有脚本，UDF或先前运行的历史记录
2. 查看当前脚本或之前的历史记录
3. 帮助程序用于帮助编写脚本
4. 脚本执行所需的参数
5. 执行按钮以运行脚本



![pig_user_view_components_hello_hdp](pictures/71)

### 创建一个新脚本

我们输入一个Pig脚本。单击视图右上角的“ **新建脚本”**按钮：

![new_script_hello_hdp_lab3](pictures/72)



将脚本命名为**riskfactor.pig**，然后单击“ **创建”**按钮：

![Lab3_7](pictures/73)





### 使用Hcatalog在Pig中加载数据

我们将使用**HCatalog**将*数据加载到Pig中*。HCatalog允许我们在Hadoop环境中*跨工具*和用户*共享模式*。它也使我们能够*分解出的模式*，并*从位置信息*提供*查询和脚本*，并*在一个共同的信息库集中他们*。由于它在HCatalog中，我们可以使用**HCatLoader()函数**。Pig允许我们为表提供名称或别名，而不必担心分配空间和定义结构。我们只需要操心我们如何处理表格即可。

- 我们可以使用位于脚本文件名称下方的Pig助手为我们提供该行的模板。单击**Pig helper -> HCatalog -> LOAD**模板
- 条目**％TABLE％**以红色突出显示。键入表的名称`geolocation`。
- 请记住在模板之前添加**a =**。这将结果保存到a中。注意**'='**必须在它之前和之后有空格。
- 我们完成的代码行如下所示：



```
a = LOAD 'geolocation' USING org.apache.hive.hcatalog.pig.HCatLoader();
```

上面的脚本在我们的例子中使用 *HCatLoader()* 函数从名为**geolocation**的文件加载数据。将上面的Pig代码复制并粘贴到riskfactor.pig窗口中。

>  注意：请参阅[Pig Latin Basics - load](http://pig.apache.org/docs/r0.14.0/basic.html#load)以了解有关**加载**运算符的更多信息。

### 过滤数据集

下一步是**选择记录的子集**，因此我们记录*了事件不正常*的司机。要在Pig中执行此操作，我们**使用Filter运算符**。我们**指示Pig过滤**我们的表并将*所有记录*保存*在event！=“normal”中*并将其存储在b中。通过这一个简单的操作，Pig将查看表中的每条记录，并过滤掉所有不符合我们标准的记录。

- 我们可以通过单击**Pig helper-> Relational Operators - > FILTER**模板再次使用Pig Help
- 我们可以用**“a”**替换**％VAR％**（提示：选项卡跳转到下一个字段）
- 我们的**％COND％**是“**event !=’normal’;** ”（注意：正常情况下需要单引号，不要忘记尾随分号）
- 完整的代码行如下所示：

```
b = filter a by event != 'normal';
```



将上面的Pig代码复制并粘贴到riskfactor.pig窗口中。

> 注意：请参阅[Pig Latin Basics - 过滤器](http://pig.apache.org/docs/r0.14.0/basic.html#filter)以了解有关**过滤器**运算符的更多信息。

### 迭代你的数据集

由于我们有正确的记录集，让我们迭代它们。我们在分组数据上使用**“foreach”**运算符来遍历所有记录。我们还想**知道与一个司机相关的非正常事件的数量**，因此为了实现这一点，我们向数据集中的*每一行添加“1”*。

- 单击**Pig helper - > Relational Operators - > FOREACH**模板再次使用Pig Help
- 我们的**％DATA％**是**b**，第二个**％NEW_DATA％**是“ **driverid，event，（int）'1'作为出现;** ”
- 完整的代码行如下所示：

```
c = foreach b generate driverid, event, (int) '1' as occurance;
```

将上面的Pig代码复制并粘贴到riskfactor.pig窗口中：

>  注意：请参阅[Pig Latin Basics - foreach](http://pig.apache.org/docs/r0.14.0/basic.html#foreach)以了解有关**foreach**运算符的更多信息。

### 计算每个司机的非正常事件总数

该**组**语句是重要的，因为它*组的记录由一个或多个关系*。在我们的例子中，我们希望按司机ID分组并再次迭代每一行以对非正常事件求和。

- 使用模板**Pig helper - > Relational Operators - > GROUP％VAR％BY％VAR％**
- 首先**％VAR％**取**“c”**，第二**％VAR％**取“ **driverid;** ”
- 完整的代码行如下所示：

```
d = group c by driverid;
```

将上面的Pig代码复制并粘贴到riskfactor.pig窗口中。

- 接下来再次使用Foreach语句添加出现。

```
e = foreach d generate group as driverid, SUM(c.occurance) as t_occ;
```

> 注意：请参阅[Pig Latin Basics - group](http://pig.apache.org/docs/r0.14.0/basic.html#group)以了解有关**组**操作员的更多信息。

### 加载DRIVERMILEAGE表并执行JOIN操作

在本节中，我们将使用**Hcatlog**将drivermileage表加载到Pig中，**并对driverid**执行**join**操作。**得到的数据**集将*给予我们, 对于一个特定的司机的总里程和总的非正常事件。

- 使用HcatLoader（）加载司机里程

```
g = LOAD 'drivermileage' using org.apache.hive.hcatalog.pig.HCatLoader();
```

- 使用模板**Pig helper - > Relational Operators-> JOIN％VAR％BY**
- 将**％VAR％**替换为' **e** '并在**BY**之后将' **driverid，g替换为driverid;** “
- 完整的代码行如下所示：

```
h = join e by driverid, g by driverid;
```

将以上两个Pig代码复制并粘贴到riskfactor.pig窗口中。

> 注意：请参阅[Pig Latin Basics - join](http://pig.apache.org/docs/r0.14.0/basic.html#join-inner)以了解有关**join**运算符的更多信息。

### 计算驾驶员风险因素

在本节中，我们将驾驶员风险因素与每个驾驶员相关联。要**计算驾驶员风险因子**，*请将非正常事件发生的总行驶里程除以*。

- 我们将再次使用**Foreach**语句来计算每个驾驶员的驾驶风险因素。

- 使用以下代码并将其粘贴到Pig脚本中。

```
final_data = foreach h generate $0 as driverid, $1 as events, $3 as totmiles, (float) $3/$1 as riskfactor;
```

- 最后一步，*使用Hcatalog* **将数据存储**到表*中*。

```
store final_data into 'riskfactor' using org.apache.hive.hcatalog.pig.HCatStorer();
```

以下是最终代码以及将其粘贴到编辑器后的样子。

> 注意：请参阅[Pig Latin Basics - store](http://pig.apache.org/docs/r0.14.0/basic.html#store)以了解有关储存操作的更多信息。

### 添加PIG参数

添加Pig参数**-useHCatalog**（区分大小写）。

![pig_script_argument](pictures/74)

**Final Pig脚本应如下所示：**

```java
a = LOAD 'geolocation' using org.apache.hive.hcatalog.pig.HCatLoader();
b = filter a by event != 'normal';
c = foreach b generate driverid, event, (int) '1' as occurance;
d = group c by driverid;
e = foreach d generate group as driverid, SUM(c.occurance) as t_occ;
g = LOAD 'drivermileage' using org.apache.hive.hcatalog.pig.HCatLoader();
h = join e by driverid, g by driverid;
final_data = foreach h generate $0 as driverid, $1 as events, $3 as totmiles, (float) $3/$1 as riskfactor;
store final_data into 'riskfactor' using org.apache.hive.hcatalog.pig.HCatStorer();
```



![riskfactor_computation_script_lab3](pictures/75)



单击左侧列中的“ **保存”**按钮保存文件。`riskfactor.pig`

## 快速回顾

在我们执行代码之前，让我们再次检查代码：

- `a =`行从HCatalog加载geolocation table表。
- `b =`行过滤掉事件不是“正常”的所有行。
- 然后我们添加一个名为occurrence的列，并设置它的值为1。
- 然后，我们通过driverid对记录进行分组，并总结每个司机的出现次数。
- 此时我们需要每个司机驾驶的里程数，因此我们加载使用Hive创建的表。
- 为了得到我们的最终结果，我们在 g 中通过driverid加入e中的事件计数和里程数据。
- 现在，通过驾驶里程除以时间数量来计算风险因素就变得非常简单了

您需要配置Pig Editor以使用HCatalog，以便Pig脚本可以加载正确的库。在Pig arguments文本框中，输入**-useHCatalog**并单击**Add**按钮：

> **请注意，**此参数**区分大小写**。它应该准确输入。`-useHCatalog`

![Lab3_9](pictures/76)

Pig View 的**Arguments**部分现在应如下所示：

![Lab3_10](pictures/77)



## 在Tez上执行Pig Script 

### 执行PIG脚本

单击**Tez上的Execute**复选框，最后点击蓝色的**Execute**按钮提交作业。Pig作业将提交给集群。这将生成一个新选项卡，其中包含Pig作业的运行状态，在顶部您将找到一个显示作业状态的进度条。

![execute_pig_script_compute_riskfactor_hello_hdp_lab3](pictures/78)

### 查看结果部分

等待工作完成。作业的输出显示在“ **结果”**部分中。请注意，您的脚本不会输出任何结果 - 它会将结果存储到Hive表中 - 因此您的Results部分将为空。

![running_script_riskfactor_hello_hdp_lab3](pictures/79)



![completed_riskfactor_script_hello_hdp_lab3](pictures/80)



单击**Logs**下拉菜单，查看脚本运行时发生的情况。错误将出现在此处。

### 查看日志部分（调试实践）

**为什么日志很重要？**

如果预期输出没有成功，在调试代码的时候 logs 部分很有用。例如，在下一节中，我们从**Riskfactor**表中加载样本数据，但没有出现任何内容。日志将告诉我们作业失败的原因。可能发生的一个常见问题是pig无法从**地理位置**表或**drivermileage**表中成功读取数据。因此，我们可以有效地解决这个问题。

让我们确认Pig从这些表中成功读取了数据, 并将数据存储到我们的**riskfactor**表中。您应该收到类似的输出：

![debug_through_logs_lab3](pictures/81)



我们的日志向我们展示了关于Pig Script的什么结果?

- 从我们的**地理位置**表中读取8000条记录
- 从我们的**drivermileage**表中读取100条记录
- 在我们的**riskfactor**表中存储了99条记录

### 验证Pig脚本成功填充的Hive表

返回Ambari Hive View 2.0并浏览`riskfactor`表中的数据以验证您的Pig作业是否已成功填充此表。这是它看起来的样子:

![pig_populated_riskfactor_table_hello_hdp_lab3](pictures/82)

此时我们现在拥有卡车每加仑平均英里数表（`avg_mileage`）和我们的风险因子表（`riskfactor`）。

## 总结

恭喜你完成本案例课程！让我们总结一下我们在本教程中学到的Pig命令，以计算地理定位和卡车数据的风险因素分析。我们学会了使用Pig来使用**LOAD {hive_table} ... HCatLoader（）**脚本从Hive访问数据。因此，我们能够执行**filter **，**foreach**，**group**，**join**和**store {hive_table} ... HCatStorer（）**脚本来操作，转换和处理这些数据。要查看这些粗体Pig Latin运算符，请查看[Pig Latin Basics](http://pig.apache.org/docs/r0.14.0/basic.html)，其中包含有关每个运算符的文档。

## 进一步阅读

加强您对Pig Latin的基础，并强化为什么这个脚本平台有利于使用这些资源处理和分析海量数据集：

- 要练习更多Pig编程，请访问[Pig Tutorials](https://zh.hortonworks.com/tutorials/?filters=apache-pig)
- [Apache Pig](https://zh.hortonworks.com/hadoop/pig/)
- [Programming Pig](http://www.amazon.com/Programming-Pig-Alan-Gates/dp/1449302645/ref=sr_1_2?ie=UTF8&qid=1455994738&sr=8-2&keywords=pig+latin&refinements=p_72%3A2661618011)
- [HDP开发者：APACHE PIG和HIVE](https://zh.hortonworks.com/training/class/hadoop-2-data-analysis-pig-hive/)

















































































































































