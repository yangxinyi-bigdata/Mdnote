

## 介绍

Hello World is often used by developers to familiarize themselves with new concepts by building a simple program. This tutorial aims to achieve a similar purpose by getting practitioners started with Hadoop and HDP. We will use an Internet of Things (IoT) use case to build your first HDP application.

This tutorial describes how to refine data for a Trucking IoT  [Data Discovery](https://zh.hortonworks.com/solutions/advanced-analytic-apps/#data-discovery) (aka IoT Discovery) use case using the Hortonworks Data Platform. The IoT Discovery use cases involves vehicles, devices and people moving across a map or similar surface. Your analysis is targeted to linking location information with your analytic data.

For our tutorial we are looking at a use case where we have a truck fleet. Each truck has been equipped to log location and event data. These events are streamed back to a datacenter where we will be processing the data.  The company wants to use this data to better understand risk.

开发人员经常使用Hello World通过构建一个简单的程序来熟悉新概念。本教程旨在通过让从业者开始使用Hadoop和HDP来实现类似的目的。我们将使用物联网（IoT）用例来构建您的第一个HDP应用程序。

本教程描述了如何使用Hortonworks数据平台优化Trucking IoT   [数据发现](https://zh.hortonworks.com/solutions/advanced-analytic-apps/#data-discovery)（也称为IoT发现）用例的数据。物联网发现用例涉及车辆，设备和人员在地图或类似表面上移动。您的分析旨在将位置信息与分析数据相关联。

在我们的教程中，我们正在研究一个我们有卡车车队的案例。每辆卡车都配备了记录位置和事件数据。这些事件将流式传输回我们将处理数据的数据中心。该公司希望使用这些数据来更好地了解风险。

以下是[分析地理位置数据](http://youtu.be/n8fdYHoEEAM)的视频，以向您展示您将在本教程中执行的操作。



## 先决条件Prerequisites

- 已下载并已安装[Hortonworks Sandbox](https://zh.hortonworks.com/downloads/#sandbox)
- 通过[学习HDP Sandbox的线索来熟悉Sandbox。
- **可选**安装[ Hortonworks ODBC驱动程序](http://zh.hortonworks.com/downloads/#addons)。[使用Microsoft Excel for Windows](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/7/)教程进行[数据报告时](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/7/)需要这样做。

## 大纲

- [加强您在Hortonworks数据平台（HDP）中的基础的概念](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/1/)
- [将传感器数据加载到HDFS中](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/2/)/Loading Sensor Data into HDFS
- [Hive - 数据ETL](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/3/)
- [Pig- 风险因素](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/4/)/Pig – Risk Factor
- [Spark  - 风险因素](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/5/)
- [使用Zeppelin进行数据报告](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/6/)
- [使用Microsoft Excel for Windows进行数据报告](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/7/)



# 概念

## 介绍

In this tutorial, we will explore important concepts that will strengthen your foundation in the Hortonworks Data Platform (HDP).

在本教程中，我们将探索重要概念, 这些概念会加强您在Hortonworks数据平台（HDP）中的基础。

Apache Hadoop是一种分层结构，用于处理和存储大量数据。

在此案例中，Apache Hadoop将是HDP形式展现的企业解决方案。

At the base of HDP exists our data storage environment known as the Hadoop Distributed File System.

在HDP的基础上存在的数据存储环境，我们将其称之为Hadoop分布式文件系统。

When data files are accessed by Hive, Pig or another coding language, YARN is the Data Operating System that enables them to analyze, manipulate or process that data.

YARN是一个数据操作系统, 通过它, 像Hive, Pig, 或者其他编程语言才能够分析, 管理或者处理数据.

HDP包括各种组件，为医疗保健，金融，保险和其他影响人们的行业提供新的机会和效率。

## 大纲

- [1.概念：Hadoop和HDP](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/1/#concepts-hadoop-hdp)
- [2.概念：HDFS](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/1/#concepts-hdfs)
- [3.概念：MapReduce和YARN](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/1/#concepts-mapreduce-yarn)
- [4.概念：Hive and Pig](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/1/#concepts-hive-pig)
- [进一步阅读](https://zh.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/section/1/#further-reading)

## 1.概念：HADOOP和HDP

在本单元中，您将了解Apache Hadoop及其扩展到大型数据集的原因。我们还将讨论Hadoop生态系统的各种组件，这些组件使Hortonworks数据平台（HDP）成为可能。

本单元讨论Apache Hadoop及其作为数据平台的功能。Hadoop的核心及其周围的生态系统解决方案供应商提供了与数据仓库和其他企业数据系统集成的企业需求。这些是实现现代数据架构和实现企业“[^数据湖]”的步骤。

[^数据湖]: 

### 1.1本模块的目标

- 了解Hadoop
- 了解HDP的五大支柱
- 了解HDP组件及其用途

### 1.2 APACHE HADOOP

Apache Hadoop是一个开源框架，用于分布式存储和处理商用硬件上的大量数据。Hadoop使企业能够从大量结构化和非结构化数据中快速获得洞察力。许多Apache Software Foundation项目构成了企业部署，集成和使用Hadoop所需的服务。有关Hadoop的更多信息，请参阅下面的博客参考。

Hortonworks博客：

- [了解Hadoop 2.0](https://zh.hortonworks.com/blog/understanding-hadoop-2-0/)
- [Apache Hadoop 3如何通过Apache Hadoop 2.0增加价值](https://zh.hortonworks.com/blog/hadoop-3-adds-value-hadoop-2/)

基本的Apache Hadoop框架由以下模块组成：

- **Hadoop Common** - 包含其他Hadoop模块所需的库和实用程序。
- **Hadoop分布式文件系统（HDFS）** - 一种分布式文件系统，可在商用计算机上存储数据，在整个群集中提供非常高的聚合带宽。
- **Hadoop YARN** - 一个资源管理平台，负责管理集群中的计算资源并将其用于用户应用程序的调度。
- **Hadoop MapReduce** - 一种用于大规模数据处理的编程模型。



每个项目都是为了提供明确的功能而开发的，每个项目都有自己的开发人员社区和个人发布周期。Hadoop有五个支柱可以让企业做好准备：

- **数据管理**  在线性扩展的存储层中存储和处理大量数据。Hadoop分布式文件系统（HDFS）是高效扩展存储层的核心技术，旨在运行低成本的商用硬件。Apache Hadoop YARN是Enterprise Hadoop的先决条件，因为它提供了资源管理和可插拔架构，使各种数据访问方法能够以可预测的性能和服务级别对存储在Hadoop中的数据进行操作。
  - [**Apache Hadoop YARN**](https://zh.hortonworks.com/hadoop/yarn) - 作为核心Hadoop项目的一部分，YARN是Hadoop数据处理的下一代框架，通过支持与其他编程模型相关的非MapReduce工作负载来扩展MapReduce功能。
  - [**HDFS**](https://zh.hortonworks.com/hadoop/hdfs/) - Hadoop分布式文件系统（HDFS）是一个基于Java的文件系统，提供可扩展且可靠的数据存储，旨在跨越大型商用服务器集群。

- **数据访问** - 以各种方式与您的数据交互 - 从批处理到实时。Apache Hive是最广泛采用的数据访问技术，尽管有许多专用引擎。例如，Apache Pig提供脚本功能，Apache Storm提供实时处理，Apache HBase提供按列的NoSQL存储，Apache Accumulo提供单元级访问控制。

  由于YARN和中间引擎（如Apache Tez用于交互式访问）和Apache Slider（用于长时间运行的应用程序），所有这些引擎都可以在一系列的数据和资源上工作。YARN还为新兴和新兴的数据访问方法提供了灵活性，例如用于搜索和编程框架（如Cascading）的Apache Solr。

  - [**Apache Hive**](https://zh.hortonworks.com/hadoop/hive) - 基于MapReduce框架，Hive是一个数据仓库，可通过类似SQL的界面, 对存储在HDFS中的大型数据集, 轻松进行数据汇总和指定查询。
  - [**Apache Pig**](https://zh.hortonworks.com/hadoop/pig) - 处理和分析大型数据集的平台。Pig由一种高级语言（Pig Latin）组成，用于表示与MapReduce框架配对的数据分析程序，用于处理这些程序。
  - [**MapReduce**](https://zh.hortonworks.com/hadoop/mapreduce/) - MapReduce是一个用于编写应用程序的框架，这些应用程序以可靠且容错的方式在数千台机器的集群中并行处理大量结构化和非结构化数据。
  - [**Apache Spark**](https://zh.hortonworks.com/hadoop/spark) - Spark是内存数据处理的理想选择。它允许数据科学家为高级分析实施快速迭代算法，例如数据集的聚类和分类。
  - [**Apache Storm**](https://zh.hortonworks.com/hadoop/storm) - Storm是一个分布式实时计算系统，用于处理快速，大量的数据流，为Apache Hadoop 2.x增加可靠的实时数据处理能力
  - [**Apache HBase**](https://zh.hortonworks.com/hadoop/hbase) - 面向列的NoSQL数据存储系统，为用户应用程序提供对大数据的随机实时读/写访问。
  - [**Apache Tez**](https://zh.hortonworks.com/hadoop/tez) - Tez将MapReduce范例概括为一个更强大的框架，用于执行近乎实时的大数据处理的复杂DAG（有向无环图）任务。
  - [**Apache Kafka**](https://zh.hortonworks.com/hadoop/kafka) - Kafka是一种快速且可扩展的发布 - 订阅消息传递系统，由于其更高的吞吐量，复制和容错能力，它经常被用来代替传统的消息代理。
  - [**Apache HCatalog**](https://zh.hortonworks.com/hadoop/hcatalog) - 一种表和元数据管理服务，为数据处理系统提供了一种集中的方式来理解Apache Hadoop中存储的数据的结构和位置。
  - [**Apache Slider**](https://zh.hortonworks.com/hadoop/slider) - 在Hadoop中部署长时间运行的数据访问应用程序的框架。Slider利用YARN的资源管理功能来部署这些应用程序，管理他们的生命周期并向上或向下扩展。
  - [**Apache Solr**](https://zh.hortonworks.com/hadoop/solr) - Solr是用于搜索存储在Hadoop中的数据的开源平台。Solr可以在世界上许多最大的互联网站点上实现强大的全文搜索和近实时索引。
  - [**Apache Mahout**](https://zh.hortonworks.com/hadoop/mahout) - Mahout为Hadoop提供可扩展的机器学习算法，有助于数据科学进行聚类，分类和基于批处理的协同过滤。
  - [**Apache Accumulo**](https://zh.hortonworks.com/hadoop/accumulo) - Accumulo是一个具有单元级访问控制的高性能数据存储和检索系统。它是Google Big Table设计的可扩展实现，可在Apache Hadoop和Apache ZooKeeper之上运行。



- **数据治理和集成** - 快速轻松地加载数据，并根据策略进行管理。Workflow Manager提供数据治理的工作流程，而Apache Flume和Sqoop可以轻松提取数据，NFS和WebHDFS接口也可以与HDFS一起使用。
  - [**工作流管理**](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.4/bk_workflow-management/content/index.html) - Workflow Manager允许您轻松创建和计划工作流并监控工作流作业。它基于Apache Oozie工作流引擎，允许用户将大数据处理任务的执行连接并自动化到定义的工作流中。
  - [**Apache Flume**](https://zh.hortonworks.com/hadoop/flume) - Flume允许您有效地聚合并将来自许多不同来源的大量日志数据移动到Hadoop。
  - [**Apache Sqoop**](https://zh.hortonworks.com/hadoop/sqoop) - Sqoop是一种加速和简化Hadoop数据[**移入**](https://zh.hortonworks.com/hadoop/sqoop)和移出的工具。它为各种流行的企业数据源提供可靠的并行负载。



- **安全性** - 地址请求的认证，授权，账户和数据的保护。Hadoop堆栈的每一层都提供安全性，从HDFS和YARN到Hive，以及其他数据访问组件通过Apache Knox提供到集群的整个周边。
  - [**Apache Knox**](https://zh.hortonworks.com/hadoop/knox) - Knox Gateway（“Knox”）为群集中的Apache Hadoop服务提供单点身份验证和访问。该项目的目标是为访问群集数据和执行作业的用户以及控制群集访问权限的操作员简化Hadoop安全性。
  - [**Apache Ranger**](https://zh.hortonworks.com/hadoop/ranger) - Apache Ranger为Hadoop集群提供了全面的安全方法。它提供跨核心企业安全性要求的中央安全策略管理，包括授权，账户和数据保护。

- **运营** - 大规模提供，管理，监控和运营Hadoop集群。
  - [**Apache Ambari**](https://zh.hortonworks.com/hadoop/ambari) - Apache Hadoop集群的开源安装生命周期管理，管理和监视系统。
  - [**Apache Oozie**](https://zh.hortonworks.com/hadoop/oozie) - 用于安排Apache Hadoop作业的Oozie Java Web应用程序。Oozie将多个作业按顺序组合成一个逻辑工作单元。
  - [**Apache ZooKeeper**](https://zh.hortonworks.com/hadoop/zookeeper) - 用于协调分布式进程的高可用性系统。分布式应用程序使用ZooKeeper来存储和调解重要配置信息的更新。

Apache Hadoop可用于跨越几乎所有垂直行业的一系列用例。在您需要存储，处理和分析大量数据的任何地方，它现在都非常流行。示例包括数字营销自动化，欺诈检测和预防，社交网络和关系分析，新药的预测建模，零售店内行为分析和基于移动设备位置的营销。要了解有关Apache Hadoop的更多信息，请观看以下介绍：

### 1.3 HORTONWORKS数据平台（HDP）

Hortonworks数据平台（HDP）是一个打包的软件Hadoop发行版，旨在简化Hadoop集群的部署和管理。与简单地下载各种Apache代码库并尝试将它们一起运行到系统相比，HDP大大简化了Hadoop的使用。HDP完全构建，开发和构建，提供企业就绪数据平台，使组织能够采用现代数据架构。

HDP 以 YARN 作为其架构中心，是一系列处理方法（从批量到交互式再到实时）的多工作负荷数据处理平台，拥有企业数据平台所需的关键能力 - 广泛的管制、安全和运营。

Hortonworks **Sandbox**是HDP的单节点实现。它被打包为虚拟机，可以快速，轻松地对HDP进行评估和实验。Sandbox中的教程和功能旨在探索HDP如何帮助您解决业务大数据问题。Sandbox教程将指导您如何将一些示例数据导入HDP以及如何使用HDP内置的工具对其进行操作。我们的想法是向您展示如何开始并向您展示如何在HDP中完成任务。HDP可以在您的企业中免费下载和使用，您可以在此处下载：[Hortonworks Data Platform](https://zh.hortonworks.com/download/)



## 2.概念：HDFS

随着数据的增长，单个物理机器的存储容量已经饱和。随着这种增长，迫切需要在不同的机器上划分数据。这种管理跨机器网络数据存储的文件系统称为分布式文件系统。[HDFS](https://zh.hortonworks.com/blog/thinking-about-the-hdfs-vs-other-storage-technologies/)是Apache Hadoop的核心组件，旨在存储具有流数据访问模式的大型文件，在商用硬件集群上运行。借助Hortonworks数据平台（HDP），HDFS现已扩展为支持  HDFS集群内的[异构存储](https://zh.hortonworks.com/blog/heterogeneous-storage-policies-hdp-2-2/)介质。

### 2.1本模块的目标

- 了解HDFS架构
- 了解Hortonworks Sandbox Amabri文件用户视图

### 2.2 HADOOP分布式文件系统

HDFS是一种分布式文件系统，用于存储大型数据文件。HDFS是一个基于Java的文件系统，提供可扩展和可靠的数据存储，旨在跨越大型商用服务器集群。HDFS已经展示了高达200 PB存储的生产可扩展性和4500个服务器的单个群集，支持近10亿个文件和块。HDFS是一种可扩展，容错的分布式存储系统，可与YARN协调的各种并发数据访问应用程序紧密配合。HDFS将在各种物理和系统环境下“正常工作”。通过在多个服务器之间分配存储和计算，组合存储资源可以随需求线性增长，同时在每个存储量保持经济性。

![HDSF_1](pictures/14)

HDFS集群由管理集群元数据的NameNode和存储数据的DataNode组成。文件和目录由NameInode在inode上表示。Inode记录属性，如权限，修改和访问时间，或命名空间和磁盘空间配额。

文件内容被拆分为大块（通常为128兆字节），文件的每个块都在多个DataNode上独立复制。这些块存储在DataNode上的本地文件系统中。

Namenode主动监视块的副本数。当由于DataNode故障或磁盘故障而丢失块的副本时，NameNode会创建该块的另一个副本。NameNode维护命名空间树以及块到DataNodes的映射，将整个命名空间映像保存在RAM中。

NameNode不直接向DataNode发送请求。它通过回复由这些DataNode发送的心跳向DataNode发送指令。说明包括以下命令：

- 将块复制到其他节点
- 删除本地块副本
- 重新注册并立即发送阻止报告，或
- 关闭节点

![HDFS_2](pictures/15)

- 有关HDFS的更多详细信息：[https](https://zh.hortonworks.com/hadoop/hdfs/)：[//hortonworks.com/hadoop/hdfs/](https://zh.hortonworks.com/hadoop/hdfs/)



借助HDP附带的[下一代HDFS数据架构](https://zh.hortonworks.com/blog/hdfs-2-0-next-generation-architecture/)，HDFS已经发展为通过热备用提供[自动故障](https://zh.hortonworks.com/blog/namenode-high-availability-in-hdp-2-0/)，具有完整的堆栈弹性。该视频提供了更加清晰的HDFS。

### 2.3 AMBARI文件在HORTONWORKS SANDBOX上的用户视图

Ambari文件用户视图

![hdfs_files_view_base_folder_concepts](pictures/16)



Ambari文件用户视图提供了用户友好的界面来上传，存储和移动数据。Hadoop中所有组件的基础是Hadoop分布式文件系统（[HDFS](https://zh.hortonworks.com/hadoop/hdfs/)）。这是Hadoop集群的基础。HDFS文件系统管理数据集在Hadoop集群中的存储方式。它负责跨数据节点分发数据，管理复制以实现冗余，以及管理任务，如添加，删除和恢复数据节点。

## 3.概念：MAPREDUCE和YARN

集群计算面临着一些挑战，例如如何持久存储数据并在节点发生故障时保持可用，或者如何在长时间运行的计算中处理节点故障。还存在网络瓶颈，这延迟了处理数据的时间。MapReduce通过使计算靠近数据提供解决方案，从而最大限度地减少数据移动。它是一种简单的编程模型，旨在通过将作业分成一组独立的任务来并行处理大量数据。

MapReduce编程的最大限制是map和reduce作业不是无状态的。这意味着Reduce作业必须先等待Map作业完成。这限制了最大并行度，因此[YARN](https://zh.hortonworks.com/blog/philosophy-behind-yarn-resource-management/)诞生为通用资源管理和分布式应用程序框架。

### 3.1模块的目标

- 了解 Map and Reduce jobs
- 了解YARN

### 3.2 Apache MapReduce

MapReduce是Hadoop数据处理引擎用于在群集周围分配工作的关键算法。MapReduce作业将大型数据集拆分为独立的块，并将它们组织成键值对，以进行并行处理。这种并行处理提高了集群的速度和可靠性，更快地返回解决方案并具有更高的可靠性。

The Map function divides the input into ranges by the InputFormat and creates a map task for each range in the input.

Map函数通过InputFormat将输入分割成序列,  同时对在input中的每一个序列创建一个Map人物。JobTracker将这些任务分发给工作节点。每个Map任务的输出被划分为每个reduce的一组键值对。

- `map(key1,value) -> list<key2,value2>`

The Reduce function then collects the various results and combines them to answer the larger problem that the master node needs to solve.

然后Reduce函数整合结果并且解决master node需要解决的巨大难题.

Each reduce pulls the relevant partition from the machines where the maps executed, then writes its output back into HDFS.

每一个Reduce从map执行的的机器中拉取相关分区, 然后把它的输出写回到HDFS中.

Thus, the reduce is able to collect the data from all of the maps for the keys and combine them to solve the problem.

因此, reduce能够从所有maps的keys中收集数据, 并且把它们联合起来用于解决问题.

- `reduce(key2, list<value2>) -> list<value3>`

The current Apache Hadoop MapReduce System is composed of the JobTracker, which is the master, and the per-node slaves called TaskTrackers. The JobTracker is responsible for *resource management* (managing the worker nodes i.e. TaskTrackers), *tracking resource consumption/availability* and also *job life-cycle management* (scheduling individual tasks of the job, tracking progress, providing fault-tolerance for tasks etc).

当前的Apache Hadoop MapReduce系统由作为主服务器的JobTracker和名为TaskTrackers的每个节点的从服务器组成。JobTracker负责*资源管理*（管理工作节点，即TaskTrackers），*跟踪资源消耗/可用性*以及*作业生命周期管理*（调度作业的各个任务，跟踪进度，为任务提供容错等）。

The TaskTracker has simple responsibilities – launch/teardown tasks on orders from the JobTracker and provide task-status information to the JobTracker periodically.

TaskTracker的职责很简单 - 启动/移除来自JobTracker的订单任务，并定期向JobTracker提供任务状态信息。

![Napri_l](pictures/17)



The Apache Hadoop projects provide a series of tools designed to solve big data problems. The Hadoop cluster implements a parallel computing cluster using inexpensive commodity hardware. The cluster is partitioned across many servers to provide a near linear scalability. The philosophy of the cluster design is to bring the computing to the data. So each datanode will hold part of the overall data and be able to process the data that it holds. The overall framework for the processing software is called MapReduce. Here’s a short video introduction to MapReduce:

Apache Hadoop项目提供了一系列旨在解决大数据问题的工具。Hadoop集群使用廉价的商用硬件实现并行计算集群。群集在许多服务器上进行分区，以提供一种接近于线性的可伸缩性。集群设计的理念是将计算带给数据。因此，每个datanode将保存整个数据的一部分，并能够处理它所拥有的数据。处理软件的总体框架称为MapReduce。以下是MapReduce的简短视频介绍：

<iframe width="500" height="281" src="https://www.youtube.com/embed/ht3dNvdNDzI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

![Napri_2](pictures/18)

### [3.3 Apache YARN](https://zh.hortonworks.com/blog/apache-hadoop-yarn-background-and-an-overview/) (YET ANOTHER RESOURCE NEGOTIATOR)

Hadoop HDFS is the data storage layer for Hadoop and MapReduce was the data-processing layer in Hadoop 1x. However, the MapReduce algorithm, by itself, isn’t sufficient for the very wide variety of use-cases we see Hadoop being employed to solve. Hadoop 2.0 presents YARN, as a generic resource-management and distributed application framework, whereby, one can implement multiple data processing applications customized for the task at hand. The fundamental idea of YARN is to split up the two major responsibilities of the JobTracker i.e. resource management and job scheduling/monitoring, into separate daemons: a global **ResourceManager** and per-application **ApplicationMaster** (AM).

Hadoop HDFS是Hadoop的数据存储层，MapReduce是Hadoop 1x中的数据处理层。但是，MapReduce算法本身并不足以满足我们看到Hadoop用于解决的各种用例。Hadoop 2.0将YARN作为通用资源管理和分布式应用程序框架呈现，从而可以实现为手头任务定制的多个数据处理应用程序。YARN的基本思想是将JobTracker的两个主要职责（即资源管理和作业调度/监控）分解为单独的守护进程：全局**ResourceManager**和每个应用程序**ApplicationMaster**（AM）。

ResourceManager和每个节点的从属节点NodeManager（NM）构成了一个新的通用**系统，**用于以分布式方式管理应用程序。

ResourceManager是在系统中的所有应用程序之间仲裁资源的最终权限。每个应用程序ApplicationMaster实际上是一个*特定*于*框架的*实体，其任务是协调来自ResourceManager的资源，并与NodeManager一起执行和监视组件任务。

ResourceManager has a pluggable Scheduler , which is responsible for allocating resources to the various running applications subject to familiar constraints of capacities, queues etc. The Scheduler is a pure scheduler in the sense that it performs no monitoring or tracking of status for the application, offering no guarantees on restarting failed tasks either due to application failure or hardware failures.

[ResourceManager](https://zh.hortonworks.com/blog/apache-hadoop-yarn-resourcemanager/)有一个可插拔的**调度程序**，它负责根据容量，队列等约束, 将资源分配给各种正在运行的应用程序。调度程序是一个*纯调度程序*，因为它不会监视或跟踪应用程序的状态，去提供由于应用程序故障或硬件故障，因此无法保证重新启动失败的任务。调度程序根据应用程序的*资源需求*执行其调度功能; 它基于**资源容器**的抽象概念来实现，**资源容器**包含内存，CPU，磁盘，网络等资源元素。

[NodeManager](https://zh.hortonworks.com/blog/apache-hadoop-yarn-nodemanager/)是每台机器的从属设备，负责启动应用程序的容器，监视其资源使用情况（CPU，内存，磁盘，网络）并将其报告给ResourceManager。

每个应用程序ApplicationMaster负责从Scheduler协商适当的资源容器，跟踪其状态并监视进度。从系统角度来看，ApplicationMaster本身作为普通*容器*运行。

这是YARN的架构视图：

![Napri_3](pictures/19)

新YARN **系统**中MapReduce的一个重要实现细节我想指出的是，我们重用了现有的MapReduce **框架**而没有进行任何大手术。这对于确保现有MapReduce应用程序和用户的**兼容性**非常重要。这是YARN的简短视频介绍。

<iframe width="500" height="281" src="https://www.youtube.com/embed/wlouNFscZS0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## 4.概念：HIVE AND PIG

### 4.1简介：APACHE HIVE

Hive是一种类似SQL的查询语言，使熟悉SQL的分析师能够对大量数据运行查询。Hive有三个主要功能：数据汇总，查询和分析。Hive提供的工具可以轻松进行数据提取，转换和加载（ETL）。

### 4.2模块的目标

- 了解Apache Hive
- 了解Apache Tez
- 了解Hortonworks Sandbox上的Ambari Hive用户视图

### [4.3 Apache Hive](https://hive.apache.org/)

数据分析师使用Hive来探索，构建和分析这些数据，然后将其转化为业务洞察力。Hive实现了一种SQL（Hive QL）近似语言，专注于分析并提供了丰富的SQL语义集，包括OLAP函数，子查询，公用表表达式等。Hive允许SQL开发人员或使用SQL工具的用户轻松查询，分析和处理存储在Hadoop中的数据.Hive还允许熟悉MapReduce框架的程序员插入他们的自定义映射器和缩减器，以执行可能不受支持的更复杂的分析。内置的语言功能。

Hive用户在[执行SQL查询](https://zh.hortonworks.com/blog/5-ways-make-hive-queries-run-faster/)时可以选择3个运行时。用户可以选择Apache Hadoop MapReduce，Apache Tez或Apache Spark框架作为执行后端。

以下是Hadoop中企业SQL的Hive的一些有利特性：

| 功能                                   | 介绍                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| Familiar                               | 使用基于SQL的语言查询数据                                    |
| Fast                                   | 交互式响应时间，甚至超过庞大的数据集                         |
| 可伸缩和可扩展/Scalable and Extensible | 随着数据种类和数量的增长，可以添加更多的商用机器，而不会相应地降低性能 |

### 4.3.1 HIVE如何工作

Hive中的表与关系数据库中的表类似，数据单元按从较大到更精细的单元的分类法进行组织。数据库由表组成，表由分区组成。可以通过简单的查询语言访问数据，Hive支持覆盖或附加数据。

在特定数据库中，表中的数据是序列化的，每个表都有一个对应的Hadoop分布式文件系统（HDFS）目录。每个表可以细分为多个分区，用于确定数据在表目录的子目录中的分布方式。分区内的数据可以进一步细分为存储桶。

### 4.3.2  HIVE的组成

- [**HCatalog**](https://cwiki.apache.org/confluence/display/Hive/HCatalog)是Hive的一个组件。它是Hadoop的表和存储管理层，使具有不同数据处理工具（包括Pig和MapReduce）的用户能够更轻松地在框架上读取和写入数据。HCatalog拥有一组关于Hadoop集群中数据的文件路径和元数据。这允许脚本，MapReduce和Tez，作业与数据位置和元数据（如模式）分离。此外，由于HCatalog还支持Hive和Pig等工具，因此可以在工具之间共享位置和元数据。使用想要集成的HCatalog外部工具的开放API（例如Teradata Aster）也可以在HCatalog中使用杠杆文件路径位置和元数据。

>  在过去，HCatalog曾经是一个独立的Apache项目。但是，在2013年3月，[HCatalog的项目](https://hive.apache.org/hcatalog_downloads.html)与Hive [合并](https://hive.apache.org/hcatalog_downloads.html)。HCatalog目前作为Hive的一部分发布。

- [**WebHCat**](https://cwiki.apache.org/confluence/display/Hive/WebHCat)提供的服务可用于运行Hadoop MapReduce（或YARN），Pig，Hive作业或使用HTTP（REST样式）接口执行Hive元数据操作。

这是关于Hive的简短视频介绍：

<iframe width="500" height="281" src="https://www.youtube.com/embed/Pn7Sp2-hUXE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### [4.3.3 Apache Tez](https://tez.apache.org/)

Apache Tez是一个可扩展的框架，用于构建高性能的批处理和交互式数据处理应用程序，由Apache Hadoop中的YARN协调。Tez通过显着提高速度来改进MapReduce，同时保持MapReduce扩展到数PB数据的能力。重要的Hadoop生态系统项目如Apache Hive和Apache Pig 都要使用Apache Tez，为更广泛的Hadoop生态系统开发的越来越多的第三方数据访问应用程序也是如此。

Apache Tez提供了一个开发人员API和框架来编写本机[YARN](https://zh.hortonworks.com/hadoop/yarn/)应用程序，这些应用程序可以桥接交互式和批处理工作负载。

它允许在连接超过数千个节点,在这些节点上使数据接入应用中, , 从而处理PB级别的数据,  Apache Tez组件库允许开发人员创建Hadoop应用程序，这些应用程序本身与Apache Hadoop YARN集成，并在混合工作负载集群中表现良好。

由于Tez具有可扩展性和可嵌入性，因此它可以自由地表达高度优化的数据处理应用程序，使其比面向终端用户的引擎（如[MapReduce](https://zh.hortonworks.com/hadoop/mapreduce/)和[Apache Spark）](https://zh.hortonworks.com/hadoop/spark/)更具优势。Tez还提供可定制的执行架构，允许用户将复杂的计算表达为数据流图，允许基于有关数据的实际信息和处理它所需的资源进行动态性能优化。

![Hive_1](pictures/20)



![Hive_2](pictures/21)



![Hive_3](pictures/22)



这是关于Tez的简短视频介绍。

<iframe width="500" height="281" src="https://www.youtube.com/embed/cPSfA1bhgVA" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### 4.3.4 STINGER和STINGER.NEXT



Stinger计划的开始使Hive能够以真正的大数据规模支持更广泛的应用案例：使其超越 Batch roots , 以支持交互式查询 - 所有这些都具有通用的SQL访问层。

Stinger.next是该计划的延续，旨在进一步提高SQL支持的[速度](https://zh.hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/)，规模和广度，以便在Hive中实现真正的实时访问，同时还支持事务功能。正如最初的Stinger计划所做的那样，这将通过熟悉的三阶段交付计划来解决，并在开放的Apache Hive社区中完全开发。

![Hive_4](pictures/23)

### 4.3.5 HORTONWORKS SANDBOX上的AMBARI HIVE VIEW 2.0

为了便于与Hive交互，我们在Hortonworks Sandbox中使用了一个名为Ambari Hive View 2.0的工具。它为Hive 2提供了一个交互式界面。我们可以创建，编辑，保存和运行查询，并让Hive使用一系列MapReduce作业或Tez作业为我们评估它们。

现在让我们打开Ambari Hive View 2.0并了解环境。转到Ambari用户视图图标并选择Hive View 2.0：

![selector_views_concepts](pictures/24)



Ambari Hive View 2.0

![ambari_hive_user_view_concepts](pictures/25)



有6个标签可与Hive View 2.0交互：

1. **QUERY**：这是上层显示的交互主界面, 主要的交互方式是编写，编辑和执行新SQL语句。

2. **JOBS**：这使您可以查看过去和当前正在运行的查询。它还允许您查看您有权查看的所有SQL查询。例如，如果您是运营商且分析师需要查询帮助，则Hadoop运营商可以使用“历史记录”功能查看从报告工具发送的查询。

3. **TABLES**：提供一个集中的地方查看，创建，删除和管理表。

4. **SAVED QUERIES**：显示当前用户保存的查询。单击查询右侧的齿轮图标以在工作表中打开已保存的查询以进行编辑或执行。您还可以从保存的列表中删除已保存的查询。

5. **UDF**：可以通过指向HDFS上的JAR文件并指示包含UDF定义的Java类路径，将用户定义的函数（UDF）添加到查询中。在此处添加UDF后，查询编辑器中将出现一个“插入UDF”按钮，您可以将UDF添加到查询中。

6. **设置**：允许您修改设置, 这些设置是会影响在Hive View中执行的查询的设置。

Apache Hive项目提供HDFS中数据的数据仓库视图。通过使用类SQL[HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)（HQL），Hive可以创建数据汇总，并对Hadoop集群中的大型数据集执行即席查询和分析。Hive的整体方法是在数据集上投影表结构，然后使用SQL对其进行操作。在文件上投影表结构的概念通常称为[Schema-On-Read](https://zh.hortonworks.com/blog/hivehcatalog-data-geeks-big-data-glue/)。由于您在HDFS中使用数据，因此可以跨所有数据节点扩展您的操作，并且可以操作庞大的数据集。

### 4.4简介：APACHE PIG

MapReduce允许您指定map和reduce函数，但是如何将数据处理纳入此模式可能有时需要您编写多个MapReduce阶段。使用Pig，数据结构更加丰富，您可以应用于数据的转换功能更强大。

### 4.4.1本模块的目标

- 了解Apache Pig
- 了解Tez上的Apache Pig
- 了解Hortonworks Sandbox上的Ambari Pig用户视图

### [4.4.2 Apache Pig](https://pig.apache.org/)

Apache Pig允许Apache Hadoop用户使用名为Pig Latin的简单脚本语言编写复杂的MapReduce转换。Pig将Pig Latin脚本转换为MapReduce，以便可以在YARN中执行，以访问存储在Hadoop分布式文件系统（HDFS）中的单个数据集。

Pig专为执行一系列数据操作而设计，使其成为三类大数据作业的理想选择：

- **提取 - 转换 - 加载（ETL）**数据管道，
- **研究**原始数据
- **迭代数据处理。**

无论用例如何，Pig都会：

| 特性     | 优点                                                         |
| -------- | ------------------------------------------------------------ |
| 扩展     | Pig用户可以创建自定义功能以满足其特定的处理要求              |
| 轻松编程 | 涉及相互关联的数据变换的复杂任务可以简化并编码为数据流序列。Pig程序完成了巨大的任务，但它们易于编写和维护。 |
| 自优化   | 由于系统自动优化Pig作业的执行，因此用户可以专注于语义。      |

有关更清晰的信息，请参阅Pig上的以下视频：

<iframe width="500" height="281" src="https://www.youtube.com/embed/PQb9I-8986s" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### 4.4.3Pig如何工作

Pig在Apache Hadoop YARN上运行，并使用MapReduce和Hadoop分布式文件系统（HDFS）。该平台的语言称为Pig Latin，它将Java MapReduce习语抽象为类似于SQL的形式。虽然SQL旨在查询数据，但Pig Latin允许您编写描述数据转换方式的数据流（例如聚合，连接和排序）。

由于Pig Latin脚本可以是图形（而不是需要单个输出），因此可以构建涉及多个输入，变换和输出的复杂数据流。用户可以使用Java，Python，Ruby或其他脚本语言编写自己的函数来扩展Pig Latin。Pig Latin有时会使用UDF（用户定义函数）进行扩展，用户可以用任何一种语言编写，然后直接从Pig Latin调用。

用户可以使用“pig”命令或“java”命令以两种模式运行Pig：

- **MapReduce模式。**这是默认模式，需要访问Hadoop集群。群集可以是伪分集的或完全分布的群集。
- **本地模式。**通过访问单台计算机，可以使用本地主机和文件系统安装和运行所有文件。

#### 4.4.4 AMBARI PIG用户对HORTONWORKS SANDBOX的观点

要访问Sandbox上的Ambari Pig用户视图，请单击右上角的用户视图图标，然后选择**Pig**：

![ambari_pig_view_concepts](pictures/26)



这将打开Ambari Pig用户界面界面。您的Pig View没有要显示的脚本，因此它将如下所示：

![pig_view_scripts_list_empty_concepts](pictures/27)

以下屏幕截图显示并描述了Pig用户视图的各种组件和功能：

1. 列出脚本，UDF和历史记录

2. “ **脚本”**选项卡打开用于编写/编辑脚本的组合框。“ **历史记录”**选项卡提供特定脚本的历史记录

3. **Pig助手**和**UDF助手**为语句，函数，I / O语句，HCatLoader（）和Python用户定义函数提供模板。

4. 允许您根据需要添加Pig参数。

5. 执行Pig脚本



![pig_view_workspace_interface_concepts](pictures/28)



## 进一步阅读

- HDFS是[Apache Hadoop](http://hadoop.apache.org/)的4个组件之一，另外3个是Hadoop Common，[Hadoop YARN](http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)和[Hadoop MapReduce](https://zh.hortonworks.com/hadoop/mapreduce/)。
- 要了解有关YARN的更多信息，请观看以下[YARN介绍视频](https://www.youtube.com/watch?v=ZYXVNxmMchc&list=PL2y_WpKCCNQc-7RJNoYym4_g7EZb3yzJW)。
- [Hadoop 2.7.0博客](https://zh.hortonworks.com/blog/apache-hadoop-2-7-0-released/)
- [了解Hadoop 2.0](https://zh.hortonworks.com/blog/understanding-hadoop-2-0/)
- [Apache Ambari](https://ambari.apache.org/)是一个面向Hadoop操作的开源社区基于Web的工具，已通过[Ambari用户视图](https://cwiki.apache.org/confluence/display/AMBARI/Views)进行扩展，以提供越来越多的开发人员工具作为用户视图。
- 请点击此链接以了解有关[HDP中包含的Ambari用户VIews的](https://zh.hortonworks.com/hadoop/ambari/)更多信息。

**Hive博客**：

- [基于成本的优化器使Apache Hive的速度提高了0.14倍](https://zh.hortonworks.com/blog/cost-based-optimizer-makes-apache-hive-0-14-more-than-2-5x-faster/)
- [使用Apache Hive和Stinger.next发现HDP 2.2：更快的SQL查询](http://www.slideshare.net/hortonworks/discoverhdp22faster-sql-queries-with-hive)
- [宣布Apache Hive 2.1](https://zh.hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/)
- [HIVE 0.14基于成本的优化器（CBO）技术概述](https://zh.hortonworks.com/blog/hive-0-14-cost-based-optimizer-cbo-technical-overview/)
- [5种方法可以让您的Hive查询运行得更快](https://zh.hortonworks.com/blog/5-ways-make-hive-queries-run-faster/)
- [保护JDBC和ODBC客户端对HiveServer2的访问](https://zh.hortonworks.com/blog/secure-jdbc-odbc-clients-access-hiveserver2/)
- [速度，规模和SQL：Stinger计划，Apache Hive 12和Apache Tez](https://zh.hortonworks.com/blog/speed-scale-sql-stinger-initiative-apache-hive-12-apache-tez/)
- [Hive / HCatalog - 数据极客和大数据胶水](https://zh.hortonworks.com/blog/hivehcatalog-data-geeks-big-data-glue/)

**Tez博客**

- [Apache Tez：Hadoop数据处理的新篇章](https://zh.hortonworks.com/blog/apache-tez-a-new-chapter-in-hadoop-data-processing/)
- [Apache Tez中的数据处理API](https://zh.hortonworks.com/blog/expressing-data-processing-in-apache-tez)

**ORC博客**

- [Apache ORC作为顶级项目启动](https://zh.hortonworks.com/blog/apache-orc-launches-as-a-top-level-project/)
- [HDP 2中的ORCFile：更好的压缩，更好的性能](https://zh.hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/)

**HDFS博客：**

- [HDP 2.2中的异构存储策略](https://zh.hortonworks.com/blog/heterogeneous-storage-policies-hdp-2-2/)
- [HDFS元数据目录解释](https://zh.hortonworks.com/blog/hdfs-metadata-directories-explained/)
- [HDFS中的异构存储](https://zh.hortonworks.com/blog/heterogeneous-storages-hdfs/)
- [HDFS 2.0下一代架构](https://zh.hortonworks.com/blog/hdfs-2-0-next-generation-architecture/)
- [NameNode HDP 2.0中的高可用性](https://zh.hortonworks.com/blog/namenode-high-availability-in-hdp-2-0/)
- [介绍... Tez：加速处理存储在HDFS中的数据](https://zh.hortonworks.com/blog/introducing-tez-faster-hadoop-processing/)
- 要了解有关HDFS的更多信息，请观看以下[HDFS介绍视频](https://www.youtube.com/watch?v=1_ly9dZnmWc)。

**YARN博客：**

- [YARN系列-1](https://zh.hortonworks.com/blog/resource-localization-in-yarn-deep-dive/)
- [YARN系列-2](https://zh.hortonworks.com/blog/apache-hadoop-yarn-hdp-2-2-substantial-step-forward-enterprise-hadoop/)



**Slider博客：**

- [宣布Apache Slider 0.60.0](https://zh.hortonworks.com/blog/announcing-apache-slider-0-60-0/)
- [使用Apache Slider向Apache Hadoop YARN启动长期运行服务](https://zh.hortonworks.com/blog/onboarding-long-running-services-apache-hadoop-yarn-using-apache-slider/)
- [使用Apache Slider在Hadoop上构建YARN应用程序：技术预览现已推出](https://zh.hortonworks.com/blog/apache-slider-technical-preview-now-available/)

**Capacity Scheduler博客：**

- [了解Apache Hadoop的Capacity Scheduler](https://zh.hortonworks.com/blog/understanding-apache-hadoops-capacity-scheduler/)
- [使用 Ambari 配置 YARN CapacityScheduler](https://zh.hortonworks.com/tutorial/configuring-yarn-capacity-scheduler-ambari/)
- [HDP 2.0中的多租户：容量调度程序和YARN](https://zh.hortonworks.com/blog/multi-tenancy-in-hdp-2-0-capacity-scheduler-and-yarn/)
- [通过YARN容量调度程序中的资源抢占实现更好的SLA](https://zh.hortonworks.com/blog/better-slas-via-resource-preemption-in-yarns-capacityscheduler/)









