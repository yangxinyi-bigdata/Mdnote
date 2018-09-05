# 使用 Hadoop 开发

## 1. HDP沙盒的学习线索

### 介绍

本教程面向在使用沙盒方面经验不足的用户



Sandbox是一个快捷的，预先配置的学习环境，包含最新版本的Apache Hadoop，特别是Hortonworks数据平台（HDP）。 Sandbox被打包在一个虚拟环境中, 这个虚拟环境可以在云端或者是你的私人电脑上运行。 Sandbox允许您自己学习和探索HDP。



### 运行前的必要条件

下载并安装[Hortonworks Sandbox](https://zh.hortonworks.com/tutorial/sandbox-deployment-and-install-guide/)



### 环境安装

这是开始使用Hortonworks Sandbox环境的管理部分, 通常只需要运行一次就可以了.



### 为你的沙盒确定网络适配器

当沙盒虚拟机被安装后, 会自动添加一个虚拟网卡, 有8中不同的网络模式, 沙盒的默认模式是NAT模式. 我们的教程将会涉及到的网络模式是: NAT模式和桥接Bridged Adapter 模式.



**Network Address Translation (NAT) 模式**

默认情况下，VM连接到NAT网络模式。 在这种情况下，用户虚拟机的IP地址会自动转换为主机的IP地址。NAT允许用户系统连接到外部网络上的外部设备，但外部设备无法访问客户系统.

另外，VirtualBox可以通过端口转发在客户端上访问所选服务。 

VirtualBox侦听主机上的某些端口，然后将到达这些端口的数据包重新发送到同一端口或不同端口上的用户虚拟机。

我们通过指定主机IP的方法, 将来自特定主机接口的所有流量转发到沙箱的客户虚拟机中.

如下图所示:

```shell
VBoxManage modifyvm "Hortonworks Sandbox HDP 2.6.5" --natpf1 "Sandbox Splash Page,tcp,127.0.0.1,1080,,1080"
.
.
.
VBoxManage modifyvm "Hortonworks Sandbox HDP 2.6.5" --natpf1 "Sandbox Host SSH,tcp,127.0.0.1,2122,,22"
```

你可以 通过VM设置然后选择网络选项卡设置网络

### 桥接网络

在此模式下，虚拟机可以直接访问主机已连接的网络。 路由器为虚拟机分配IP地址。 在该网络上，现在客户端IP地址可见，而不是仅显示主机IP地址。 因此，外部设备（例如在Raspberry Pi上运行的MiNiFi）能够通过其IP地址连接到虚拟机。

在桥接模式中, 在主机连接之后, 虚拟机可以直接进入网络.

路由器给虚拟机设置一个IP独立地址. 在这个网络中, 虚拟机的IP地址也是可见的, 而不是只显示主机的IP地址.

因此, 像运行在Raspberry Pi上的MiNiFi等外部设备, 可以通过它自己的IP连接到虚拟机.

我们通常什么时候需要这种模式呢? 对于连接数据结构来讲, 它是很有必要的. 

 要配置此模式，请首先需要关闭guest虚拟机, 单击设置，切换到网络选项卡，然后将连接到网络更改为桥接适配器。



![img](pictures/1)



> 警告：首先确保您的计算机已连接到路由器，否则此功能将无法工作，因为没有路由器为guest虚拟机vm分配IP地址。



### 确定你的沙盒的IP地址

Once the Sandbox VM or container is installed, it settles to the host of your environment, the IP address varies depending on your Virtual Machine (VMware, VirtualBox) or container (Docker). Once the sandbox is running, it will tell you the IP address. An example of typical IP addresses for each supported environment:

安装Sandbox VM或容器后，它将安装到您的环境主机，IP地址因您的虚拟机（VMware，VirtualBox）或容器（Docker）而异。 沙箱运行后，它会告诉您IP地址。 每个受支持环境的典型IP地址示例：



安装Sandbox VM后，它将安装到您的主机.IP地址的变化一来与你的虚拟机运行软件 (VMware, VirtualBox) .

一旦沙箱运行起来了, 它就会告诉你IP地址. 

下面是每种支持环境下通常的ip地址格式

**Docker**: IP Address = **127.0.0.1**

**VirtualBox**: IP Address = **127.0.0.1**

**VMWare**: IP Address = **192.168.x.x**



If you’re using **VirtualBox** or **VMWare**, you can confirm the IP address by waiting for the installation to complete and confirmation screen will tell you the IP address your sandbox resolves to. For example:

如果您使用的是VirtualBox或VMWare，则可以通过等待安装完成来确认IP地址，确认屏幕将告诉您沙盒解析的IP地址。 例如：



![img](pictures/2)



> Window NAT模式沙盒



![img](pictures/3)



> BRIDGED Sandbox 桥接沙盒

> **Note:** If you’re using Azure, your IP address is located on the dashboard, refer to [**Set a Static IP**](https://zh.hortonworks.com/tutorial/sandbox-deployment-and-install-guide/section/4/#set-a-static-ip)
>
> 如果你用微软Microsoft *Azure* 云计算平台, 你的IP地址会放在仪表板中

将SANDBOX IP映射到主机文件中所需的主机名

Mac，Linux和Windows都有一个hosts 文件。 配置此文件后，沙箱的IP地址将映射到hostname主机名中.

**Mac users**:

- `echo '{IP-Address} sandbox.hortonworks.com sandbox-hdp.hortonworks.com sandbox-hdf.hortonworks.com' | sudo tee -a/private/etc/hosts`

**Linux users**:

- `echo '{IP-Address} sandbox.hortonworks.com sandbox-hdp.hortonworks.com sandbox-hdf.hortonworks.com' | sudo tee -a/etc/hosts`

**Windows users**:

- Run Notepad as **administrator**.
- Open **hosts** file located in: `c:\Windows\System32\drivers\etc\hosts`
- Add `{IP-Address} localhost sandbox.hortonworks.com sandbox-hdp.hortonworks.com sandbox-hdf.hortonworks.com`
- Save the file



> 重要提示: 使用沙盒IP地址替换 **{IP-Address}**



### 终端访问

有关用户和密码的列表，请参阅登录账户。 您也可以使用root登录，使用密码hadoop，切记您需要改变成自己的密码.

如果使用root以外的账户登录，则可能需要在命令之前使用sudo。 例如：sudo ambari-server status。



#### 安全 SHELL 方法登陆:

打开终端（mac / linux）或Git Bash（Windows）。 键入以下命令以通过ssh user @ hostname -p port访问Sandbox。 

For example: `ssh root@sandbox-hdp.hortonworks.com -p 2222`

![img](pictures/4)

#### shell网络客户端方法登陆

shell Web客户端也称为shell-in-a-box。 这是一种轻松发布shell命令的方法，无需安装其他软件。 它使用端口4200，例如：sandbox-hdp.hortonworks.com:4200



#### 在沙箱和本地计算机之间发送数据

使用您选择的终端，您可以将文件传输到沙箱和本地计算机。

- 将文件从本地计算机传输到沙箱：
  - `scp -P 2222 <local_directory_file> root@sandbox-hdp.hortonworks.com:<sandbox_directory_file>`

- 将文件从沙箱传输到本地计算机：
  - `scp -P 2222 root@sandbox-hdp.hortonworks.com:<sandbox_directory_file> <local_directory_file>`

你注意到两个命令之间的区别吗？

要将数据从本地计算机发送到沙箱，本地计算机目录路径位于沙箱目录之前。要将数据从沙箱传输到本地计算机，命令参数将反转。

## 欢迎页面

Sandbox欢迎页面也称为**Splash页面**。它运行在端口号**：8888**。要打开它，请使用您的主机地址并附加端口号。例如：[http](http://sandbox-hdp.hortonworks.com:8888/)：[//sandbox-hdp.hortonworks.com](http://sandbox-hdp.hortonworks.com:8888/)：[8888 /](http://sandbox-hdp.hortonworks.com:8888/)

欢迎页面看起来大概是这样的:

![new_splashscreen](pictures/5)

通过**Launch Dashboard**(运行管理面板) 可以打开两个窗口, Ambari 的交互页面和初学者向导页面, 您应该根据教程中给的使用用户名和密码登录Ambari。

**Advanced HDP Quick Links**可快速访问Ambari服务，如Zeppelin，Atlas，Ranger，Shell-in-a-box等。

## 探索AMBARI

- Ambari Dashboard在端口上运行**：8080**。例如，[http：//sandbox-hdp.hortonworks.com：8080](http://sandbox-hdp.hortonworks.com:8080/)
- 以**管理员**身份登录，请参阅[管理密码重置](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#admin-password-reset)
- Select **Manage Ambari**

![ç®¡ç-ambari](pictures/6)

将显示以下屏幕：

![Lab0_3](pictures/7)

1. “ **Operate Your Cluster操作您的群集** ”将带您进入Ambari仪表板，这是Hadoop操作员的主要UI
2. “ **Manage Users + Groups管理用户+组** ”允许您添加和删除Ambari用户和组
3. “ **Clusters群集** ”允许您向Ambari用户和组授予权限
4. “ **Ambari User ViewsAmbari用户视图** ”列出了属于群集的Ambari用户视图集
5. “ **Deploy Views部署视图** ”提供了添加和删除Ambari用户视图的管理



- 点击**Go to Dashboard**，你会看到一个类似的屏幕：

![Lab0_4](pictures/8)



通过点击进行探索学习:

1. **Metrics**, **Heatmaps** and **Config History** 

   > 指标, 热力图, 和配置历史

2. **Dashboard**, **Services**, **Hosts**, **Alerts**, **Admin** and User Views icon (represented by 3×3 matrix ) to become familiar with the Ambari resources available to you.

   > 2. **仪表板**，**服务**，**主机**，**警报**，**管理员**和用户视图图标（由3×3矩阵表示），以熟悉您可以使用的Ambari资源。

## 探索AZURE中的SANDBOX

与[欢迎页面](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#welcome-page)类似，我们将端口号**：8888**附加到Azure上的主机地址。在Azure中部署HDP Sandbox时会生成公共IP地址。记下这个IP地址。在此示例中，IP地址为23.99.9.232。您的机器将具有不同的IP地址。

![Sandbox Welcome Screen Azure](pictures/9)

### 多种方式执行终端命令

#### SECURE SHELL (SSH) 方法：

打开你的终端（mac和linux）或putty（windows）。这也是`host`Azure提供的公共IP地址。提供在Azure上部署沙箱时提供的用户名和密码。使用以下命令通过SSH访问Sandbox：

```
# Usage:
      ssh <username>@<host> -p 22;
```

![Macç"ç"¯SSH Azure 1](pictures/10)

> Mac OS终端。键入密码时，该条目不会在屏幕上回显，它会隐藏用户输入。小心输入正确的密码。

#### SHELL WEB客户端方法：

打开Web浏览器。将以下文本替换`host`为您的浏览器以通过shell访问Sandbox。提供在Azure上部署沙箱时提供的相同用户名和密码。

```
# Usage:
    #  _host_:4200
```

![Shellæµè§å¨Sandbox Azureä¸­çShell](pictures/11)

### 手动设置AMBARI管理员密码

1.打开终端（mac或linux）或putty（windows）

2.使用您在Azure上创建沙箱时提供的用户名和密码，SSH进入沙箱。你的`host`是Azure提供的公共IP地址，sudo密码是沙盒密码。

```bash
# Usage:
      ssh <username>@<host> -p 22;
```

3.键入以下命令：

```bash
# Updates password
sudo ambari-admin-password-reset
# If Ambari doesn't restart automatically, restart ambari service
ambari-agent restart
```

## 附录A：参考表

### 登录信息：

| 用户       | 密码                                                         |
| ---------- | ------------------------------------------------------------ |
| root       | hadoop 请参阅[管理员密码重置](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#admin-password-reset) |
| maria_dev  | maria_dev                                                    |
| raj_ops    | raj_ops                                                      |
| holger_gov | holger_gov                                                   |
| amy_ds     | amy_ds                                                       |

1. **admin** - 系统管理员

2. **maria_dev** - 主观准备和洞察数据。她热爱探索不同的HDP组件，如Hive，Pig，HBase。

3. **raj_ops** - 负责基础设施建设，研究和开发活动，如设计，安装，配置和管理。他是复杂操作系统系统管理领域的技术专家。

4. **holger_gov** - 主要用于管理数据元素，包括内容和元数据。他的专业职位包括管理组织的全部数据的流程，政策，指南和责任，以遵守政策和/或监管义务。

5. **amy_ds** - 使用Hive，Spark和Zeppelin进行探索性数据分析，数据清理和转换作为分析准备的数据科学家。

下面提到了Sandbox中这些用户之间的一些显着差异：

| NAME ID(S)          | ROLE                      | 服务                                                         |
| ------------------- | ------------------------- | ------------------------------------------------------------ |
| Sam Admin           | Ambari Admin              | Ambari                                                       |
| Raj (raj_ops)       | Hadoop Warehouse Operator | Hive/Tez, Ranger, Falcon, Knox, Sqoop, Oozie, Flume, Zookeeper |
| Maria (maria_dev)   | Spark and SQL Developer   | Hive, Zeppelin, MapReduce/Tez/Spark, Pig, Solr, HBase/Phoenix, Sqoop, NiFi, Storm, Kafka, Flume |
| Amy (amy_ds)        | Data Scientist            | Spark, Hive, R, Python, Scala                                |
| Holger (holger_gov) | 数据专员                  | Atlas                                                        |

**OS Level Authorization**/操作系统授权级别

| NAME ID(S)          | HDFS AUTHORIZATION                                           | AMBARI AUTHORIZATION  | RANGER AUTHORIZATION |
| ------------------- | ------------------------------------------------------------ | --------------------- | -------------------- |
| Sam Admin           | Max Ops                                                      | Ambari Admin          | Admin access         |
| Raj (raj_ops)       | Access to Hive, Hbase, Atlas, Falcon, Ranger, Knox, Sqoop, Oozie, Flume, Operations | Cluster Administrator | Admin Access         |
| Maria (maria_dev)   | Access to Hive, Hbase, Falcon, Oozie and Spark               | Service Operator      | Normal User Access   |
| Amy (amy_ds)        | Access to Hive, Spark and Zeppelin                           | Service Operator      | Normal User Access   |
| Holger (holger_gov) | Access to Atlas                                              | Service Administrator | Normal User Access   |

**其他差异**

| NAME ID(S)          | SANDBOX ROLE          | VIEW CONFIGURATIONS | START/STOP/RESTART SERVICE | MODIFY CONFIGURATIONS | ADD/DELETE SERVICES | INSTALL COMPONENTS | MANAGE USERS/GROUPS | MANAGE AMBARI VIEWS | ATLAS UI ACCESS | [Sample Ranger Policy Access](https://zh.hortonworks.com/tutorial/tag-based-policies-with-apache-ranger-and-apache-atlas/#sample-ranger-policy) |
| ------------------- | --------------------- | ------------------- | -------------------------- | --------------------- | ------------------- | ------------------ | ------------------- | ------------------- | --------------- | ------------------------------------------------------------ |
| Sam Admin           | Ambari Admin          | 含                  | 含                         | 含                    | 含                  | 含                 | 含                  | 含                  | 含              | NA                                                           |
| Raj (raj_ops)       | Cluster Administrator | 含                  | 含                         | 含                    | 含                  | 含                 | 否                  | 否                  | 否              | 全部                                                         |
| Maria (maria_dev)   | Service Operator      | 含                  | 含                         | 否                    | 否                  | 否                 | 否                  | 否                  | 否              | SELECT                                                       |
| Amy (amy_ds)        | Service Operator      | 含                  | 含                         | 否                    | 否                  | 否                 | 否                  | 否                  | 否              | SELECT                                                       |
| Holger (holger_gov) | Service Administrator | 含                  | 含                         | 含                    | 否                  | 否                 | 否                  | 否                  | 含              | SELECT, CREATE, DROP                                         |

### SANDBOX VERSION

当您遇到问题时，有人会问的第一件事就是“ *您使用的沙箱版本是什么* ”？要获取此信息：

使用[shell Web客户端](http://sandbox-hdp.hortonworks.com:4200/)登录并执行：。输出应该类似于：`sandbox-version`

![æ²çç](pictures/12)

> 注意：请参阅[登录凭据](https://zh.hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox/#login-credentials)

### 管理员密码重置

由于密码容易被黑客攻击，我们建议 您将Ambari管理员密码更改为唯一。

1. 打开[shell Web客户端](http://sandbox-hdp.hortonworks.com:4200/)（也称为shell-in-a-box）：
2. 使用凭据登录：**root** / **hadoop**
3. 输入以下命令： `ambari-admin-password-reset`

> 重要提示：首次以**root用户**身份登录时，可能需要更改密码 

## 附录B：故障排除

- [Hortonworks社区连接](https://zh.hortonworks.com/community/forums/)（HCC）是一个很好的资源，可以找到您在Hadoop旅程中可能遇到的问题的答案。

- **挂起** / **长时间运行的进程**

有时您可能会遇到一个似乎永远无法运行但未完成的作业，查询或请求。可能是因为它处于**ACCEPTED**状态。开始查找的好地方是在[ResourceManager中](http://sandbox-hdp.hortonworks.com:8088/)。如果你知道一份工作已经完成，但是资源管理器仍然认为它正在运行 - 杀了它！

![RM-æ](pictures/13)

