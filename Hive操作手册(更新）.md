# Hive操作手册

## 一、Hive安装

### 1.Hive简介

​	Hive是Facebook开发的构建于Hadoop集群之上的数据仓库应用，可以将结构化的数据文件映射为一张数据库表，并提供完整的SQL查询功能，可以将SQL语句转换为MapReduce任务进行运行。

​	Hive是一个可以提供有效的、合理的且直观的组织和使用数据的模型，即使对于经验丰富的Java开发工程师来说，将这些常见的数据运算对应到底层的MapReduce Java API也是令人敬畏的。Hive可以帮用户做这些工作，用户就可以集中精力关注查询本身了。Hive可以将大多数的查询转换为MapReduce任务。Hive最适合于数据仓库应用程序，使用该应用程序进行相关的静态数据分析，不需要快速响应给出结果，而且数据本身也不会频繁变化。

​	Hive不是一个完整的数据库。Hadoop以及HDFS的设计本身约束和局限性限制了Hive所能胜任的工作。最大的限制就是Hive不支持记录级别的更新、插入或者删除。用户可以通过查询生成新表或将查询结果导入到文件中去。因为，Hadoop是一个面向批处理的系统，而MapReduce启动任务启动过程需要消耗很长时间，所以Hive延时也比较长。Hive还不支持事务。因此，Hive不支持联机事务处理（OLTP），更接近于一个联机分析技术（OLAP）工具，但是，目前还没有满足“联机”部分。

​	Hive提供了一系列的工具，可以用来进行数据提取转化加载(ETL)，其中，ETL是一种可以存储、查询和分析存储在Hadoop中的大规模数据的机制。因此，Hive是最适合数据仓库应用程序的，它可以维护海量数据，而且可以对数据进行挖掘，然后形成意见和报告等。

​	因为大多数的数据仓库应用程序是基于SQL的关系数据库现实的，所以，Hive降低了将这些应用程序移植到Hadoop上的障碍。如果用户懂得SQL，那么学习使用Hive会很容易。因为Hive定义了简单的类SQL 查询语言——HiveQL，这里值得一提的是，与SQLServer、Oracle相比，HiveQL和MySQL提供的SQL语言更接近。同样的，相对于其他的Hadoop语言和工具来说，Hive也使得开发者将基于SQL的应用程序移植到Hadoop变得更加容易。

### 2.Hive安装

​	接下来，开始Hive的安装，安装Hive之前，首先需要装好Hadoop和Spark。在**[Hive官网](https://hive.apache.org/)**可下载最新版本Hive，并且能够查阅版本改动说明，本次课程采用1.2.2版本进行安装。可以采用WinSCP传输apache-hive-1.2.2-bin.tar至虚拟机“下载”文件夹中，再进行后续安装。

```shell
cd ~/下载                                              # 进入下载文件夹
sudo tar -zxf apache-hive-1.2.2-bin.tar.gz -C /usr/local    # 安装至/usr/local文件夹内
cd /usr/local                                         # 进入/usr/local文件夹
sudo mv ./apache-hive-1.2.2-bin/ ./hive               # 更名为hive
sudo chown -R hadoop ./hive                           # 修改hive权限
mkdir -p /usr/local/hive/warehouse                    # 创建元数据存储文件夹
sudo chmod a+rwx /usr/local/hive/warehouse            # 修改文件权限  
```

然后添加Hive安装路径至系统环境变量

```sh
vim ~/.profile
```

添加下述路径

```shell
#Hive
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
```

并使之生效

```shell
source ~/.profile
```

修改hive读取spark的jar包地址

```shell
cd /usr/local/hive/bin
vim hive
```

修改为

```sh
# add Spark assembly jar to the classpath
if [[ -n "$SPARK_HOME" ]]
then
  sparkAssemblyPath=`ls ${SPARK_HOME}/jars/*.jar`
  CLASSPATH="${CLASSPATH}:${sparkAssemblyPath}"
fi
```

然后采用hive默认配置

```shell
cd /usr/local/hive/conf
cp hive-default.xml.template hive-default.xml
```

尝试启动Hive，此时启动是以本地模式进行启动，能正常启动则说明安装成功。

```shell
start-all.sh
hive
```

​	若出现jline等jar包错误，则需要进入到hadoop安装目录下的share/hadoop/yarn/lib下删除jline-0.9.94.jar文件，再启动hive即可（因为高版本的Hadoop对Hive有捆绑）。

```shell
cd /usr/local/hadoop/share/hadoop/yarn/lib
rm -rf jline-0.9.94.jar 
```

###3. Hive的基本配置

​	在安装Hive时，默认情况下，元数据存储在Derby数据库中。Derby是一个完全用Java编写的数据库，所以可以跨平台，但需要在JVM中运行 。因为多用户和系统可能需要并发访问元数据存储，所以默认的内置数据库并不适用于生产环境。任何一个适用于JDBC进行连接的数据库都可用作元数据库存储，这里我们把MySQL作为存储元数据的数据库。接下来，我们分别对这两种方式进行介绍，即使用Derby数据库的方式和使用MySQL数据库的方式。

#### 3.1 使用Derby作为元数据库

​	本地模式中，用户的“表”等元数据信息，都默认存储在file://user/hive/warehouse，对于其他模式默认存储路径是hdfs://namenode_server/user/hive/warehouse。使用如下命令编辑hive-site.xml文件：

```shell
vim /usr/local/hive/conf/hive-site.xml  
```

在hive-site.xml文件添加以下内容：

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/usr/local/hive/warehouse</value>   
    <description>location of default database for the warehouse</description>
  </property>
<property>
    <name>javax.jdo.option.ConnectionURL</name>
     <value>jdbc:derby:;databaseName=/usr/local/hive/metastore_db;create=true</value>                           
    <description>JDBC connect string for a JDBC metastore</description>
  </property>
</configuration>
```

​	若要以伪分布式模式和分布式模式配置Hive，只需根据Hadoop配置文件core-site.xml中fs.defaultFS的值对hive.metastore.warehouse.dir 进行相应修改即可。配置完成之后即可启动Hive，然后尝试使用HiveQL命令创建表。

```sh
hive
```

```sql
show databases;
create database if not exists derby;
use derby;
create table x(a int);
select * from x;
drop table x;
exit;
```

#### 3.2 使用MySQL作为元数据库

##### 3.2.1 安装MySQL

首先，查看并卸载系统自带的MySQL相关安装包（或之前安装过MySQL），命令如下：

```shell
sudo apt install rpm
rpm -qa | grep mysql
```

若没有安装rpm工具，系统会有提示，按照提示安装即可。接下来查看是否有系统自带的MySQL相关安装包，若有，按下面命令删除：

```shell
sudo rpm -e --nodeps mysql-libs-xxxxxx       
```

注：xxxxx是已经安装的mysql的版本号，然后进行MySQL的安装

```shell
sudo apt-get install mysql-server        
```

安装完成后，启动设置MySQL服务

```shell
sudo service mysql start
mysql -u root -p
```

当然，还可使用下列命令进行额外设置

```shell
sudo chkconfig mysql on                              # 设置开机自动启动
sudo /usr/bin/mysqladmin -u root password '123'      # 设置root用户密码
```

接下来，创建hive用户及其数据库等，用于存放Hive的元数据

```mysql
create database hive;
grant all on *.* to hive@localhost identified by 'hive'; 
flush privileges;
exit;
```

切换hive用户登陆

```shell
mysql -u hive -p hive
```

```mysql
show databases;
```

若能看到hive数据库存在，则说明创建成功。

##### 3.2.2 修改Hive配置

接下来，修改hive-site.xml文件

```shell
vim /usr/local/hive/conf/hive-site.xml  
```

输入下列信息

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
    <description>JDBC connect string for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>hive</value>
    <description>username to use against metastore database</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>hive</value>
    <description>password to use against metastore database</description>
  </property>
</configuration>
```

或者指定元数据文件夹

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/usr/local/hive/warehouse</value>   
    <description>location of default database for the warehouse</description>
  </property>
<property>
    <name>javax.jdo.option.ConnectionURL</name>
   <value>jdbc:mysql://localhost:3306/hive;createDatebaseIfNotExist=true</value>                           
    <description>JDBC connect string for a JDBC metastore</description>
  </property>
<property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
  </property>
<property> 
   <name>javax.jdo.option.ConnectionPassword </name> 
   <value>hive</value> 
</property> 
 <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>hive</value>
    <description>Username to use against metastore database</description>
   </property>
</configuration>
```

然后将JDBC文件放到hive的lib文件夹内，JDBC包的下载参考前述部分

```shell
cd ~/下载
cp mysql-connector-java-5.1.26-bin.jar /usr/local/hive/lib
mkdir -p /usr/local/hive/tmp
sudo chmod a+rwx /usr/local/hive/tmp
```

也可从官网直接下载最新版jdbc

```shell
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz
```

然后进行解压安装。当然，如果之前删除了jline-0.9.94.jar，此时需要把hive对应的jar包放进去

```shell
cp /usr/local/hive/lib/jline-2.12.jar  /usr/local/hadoop/share/hadoop/yarn/lib
```

然后尝试启动hive

```shell
schematool -dbType mysql -initSchema
start-all.sh
hive
```

成启动后，即可输入hive --help查看hive常用命令。

## 二、Hive使用

### 1.Hive基本数据类型

首先，我们简单叙述一下HiveQL的基本数据类型。

Hive支持基本数据类型和复杂类型, 基本数据类型主要有数值类型(INT、FLOAT、DOUBLE ) 、布尔型和字符串, 复杂类型有三种:ARRAY、MAP 和 STRUCT。

#### 1.1 基本数据类型

- TINYINT: 1个字节
- SMALLINT: 2个字节
- INT: 4个字节
- BIGINT: 8个字节
- BOOLEAN: TRUE/FALSE 
- FLOAT: 4个字节，单精度浮点型
- DOUBLE: 8个字节，双精度浮点型STRING 字符串

#### 1. 2 复杂数据类型

- ARRAY: 有序字段
- MAP: 无序字段
- STRUCT: 一组命名的字段

### 2.常用的HiveQL操作命令

​	Hive常用的HiveQL操作命令主要包括：数据定义、数据操作。接下来详细介绍一下这些命令即用法（想要了解更多请参照《Hive编程指南》一书）。

#### 2.1 数据定义

主要用于创建修改和删除数据库、表、视图、函数和索引。

- 创建、修改和删除数据库

  ```sql
  create database if not exists hive;       #创建数据库
  show databases;                           #查看Hive中包含数据库
  show databases like 'h.*';                #查看Hive中以h开头数据库
  describe databases;                       #查看hive数据库位置等信息
  alter database hive set dbproperties;     #为hive设置键值对属性
  use hive;                                 #切换到hive数据库下
  drop database if exists hive;             #删除不含表的数据库
  drop database if exists hive cascade;     #删除数据库和它中的表
  ```

  注意，除 dbproperties属性外，数据库的元数据信息都是不可更改的，包括数据库名和数据库所在的目录位置，没有办法删除或重置数据库属性。

- 创建、修改和删除表

  ```sql
  #创建内部表（管理表）
  create table if not exists hive.usr(
        name string comment 'username',
        pwd string comment 'password',
        address struct<street:string,city:string,state:string,zip:int>,
        comment  'home address',
        identify map<int,tinyint> comment 'number,sex') 
        comment 'description of the table'  
       tblproperties('creator'='me','time'='2016.1.1'); 
  #创建外部表
  create external table if not exists usr2(
        name string,
        pwd string,
    address struct<street:string,city:string,state:string,zip:int>,
        identify map<int,tinyint>) 
        row format delimited fields terminated by ','
       location '/usr/local/hive/warehouse/hive.db/usr'; 
  #创建分区表
  create table if not exists usr3(
        name string,
        pwd string,
        address struct<street:string,city:string,state:string,zip:int>,
        identify map<int,tinyint>) 
        partitioned by(city string,state string);    
  #复制usr表的表模式  
  create table if not exists hive.usr1 like hive.usr;
   
  show tables in hive;  
  show tables 'u.*';        #查看hive中以u开头的表
  describe hive.usr;        #查看usr表相关信息
  alter table usr rename to custom;      #重命名表
   
  #为表增加一个分区
  alter table usr2 add if not exists 
       partition(city=”beijing”,state=”China”) 
       location '/usr/local/hive/warehouse/usr2/China/beijing'; 
  #修改分区路径
  alter table usr2 partition(city=”beijing”,state=”China”)
       set location '/usr/local/hive/warehouse/usr2/CH/beijing';
  #删除分区
  alter table usr2 drop if exists  partition(city=”beijing”,state=”China”)
  #修改列信息
  alter table usr change column pwd password string after address;
   
  alter table usr add columns(hobby string);                  #增加列
  alter table usr replace columns(uname string);              #删除替换列
  alter table usr set tblproperties('creator'='liming');      #修改表属性
  alter table usr2 partition(city=”beijing”,state=”China”)    #修改存储属性
  set fileformat sequencefile;             
  use hive;                                                   #切换到hive数据库下
  drop table if exists usr1;                                  #删除表
  drop database if exists hive cascade;                       #删除数据库和它中的表
  ```

- 视图和索引的创建、修改和删除

  基本语法格式

  ```sql
  create view view_name as....;                #创建视图
  alter view view_name set tblproperties(…);   #修改视图
  ```

  因为视图是只读的，所以 对于视图只允许改变元数据中的 tblproperties属性。

  ```sql
  #删除视图
  drop view if exists view_name;
  #创建索引
  create index index_name on table table_name(partition_name/column_name)  
  as 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler' with deferred rebuild....; 
  ```

  这里’org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler’是一个索引处理器，即一个实现了索引接口的Java类，另外Hive还有其他的索引实现。

  ```sql
  alter index index_name on table table_name partition(...)  rebulid;   #重建索引
  ```

  如果使用 deferred rebuild，那么新索引成空白状态，任何时候可以进行第一次索引创建或重建。

  ```sql
  show formatted index on table_name;                       #显示索引
  drop index if exists index_name on table table_name;      #删除索引
  ```

#### 2.2 数据操作

主要实现的是将数据装载到表中（或是从表中导出），并进行相应查询操作

- 向表中装载数据

  ```sql
  create table if not exists hive.stu(id int,name string) 
  row format delimited fields terminated by '\t';
  create table if not exists hive.course(cid int,sid int) 
  row format delimited fields terminated by '\t';
  ```

  向表中装载数据有两种方法：从文件中导入和通过查询语句插入。

  - 从文件中导入

    假如这个表中的记录存储于文件stu.txt中，该文件的存储路径为usr/local/hadoop/examples/stu.txt，内容如下。

    ```sh
    1 Hello
    2 World
    3 CDA
    ```

    ```sql
    load data local inpath '/usr/local/hadoop/examples/stu.txt' overwrite into table stu;
    ```

  - 通过查询语句插入

    使用如下命令，创建stu1表，它和stu表属性相同，我们要把从stu表中查询得到的数据插入到stu1中：

    ```sql
    create table stu1 as select id,name from stu;
    ```

    上面是创建表，并直接向新表插入数据；若表已经存在，向表中插入数据需执行以下命令：

    ```sql
    insert overwrite table stu1 select id,name from stu where（条件）;
    ```

    这里关键字overwrite的作用是替换掉表（或分区）中原有数据，换成into关键字，直接追加到原有内容后。

- 写入临时文件

  ```sql
  insert overwrite local directory '/usr/local/hadoop/tmp/stu'  select id,name from stu;
  ```


- 查询操作

  ```sql
  select id,name,
    case 
    when id=1 then 'first' 
    when id=2 then 'second'
    else 'third'
  ```

#### 2.3 连接

​	连接（join）是将两个表中在共同数据项上相互匹配的那些行合并起来, HiveQL 的连接分为内连接、左向外连接、右向外连接、全外连接和半连接 5 种。

- 内连接(等值连接)

  内连接使用比较运算符根据每个表共有的列的值匹配两个表中的行。

  首先，我们先把以下内容插入到course表中（自行完成）。

```
1 3
2 1
3 1
```

​	下面, 查询stu和course表中学号相同的所有行，命令如下：

```sql
select stu.*, course.* from stu join course on(stu .id=course .sid);
```



- 左连接

  ​	左连接的结果集包括“LEFT OUTER”子句中指定的左表的所有行, 而不仅仅是连接列所匹配的行。如果左表的某行在右表中没有匹配行, 则在相关联的结果集中右表的所有选择列均为空值，命令如下：

```sql
select stu.*, course.* from stu left outer join course on(stu .id=course .sid); 
```

- 右连接

  ​	右连接是左向外连接的反向连接,将返回右表的所有行。如果右表的某行在左表中没有匹配行,则将为左表返回空值。命令如下：

```sql
select stu.*, course.* from stu right outer join course on(stu .id=course .sid); 
```

- 全连接

  ​	全连接返回左表和右表中的所有行。当某行在另一表中没有匹配行时,则另一个表的选择列表包含空值。如果表之间有匹配行,则整个结果集包含基表的数据值。命令如下：

```sql
select stu.*, course.* from stu full outer join course on(stu .id=course .sid); 
```

- 半连接

  ​	半连接是 Hive 所特有的, Hive 不支持 in 操作,但是拥有替代的方案; left semi join, 称为半连接, 需要注意的是连接的表不能在查询的列中,只能出现在 on 子句中。命令如下：

```sql
select stu.* from stu left semi join course on(stu .id=course .sid); 
```



## 三、Spark与Hive集成

### 1.安装Spark

​	为了让Spark能够访问Hive，必须为Spark添加Hive支持。Spark官方提供的预编译版本，通常是不包含Hive支持的，需要采用源码编译，编译得到一个包含Hive支持的Spark版本。首先测试一下电脑上已经安装的Spark版本是否支持Hive

```sh
spark-shell
```

这样就启动进入了spark-shell，然后输入：

```scala
import org.apache.spark.sql.hive.HiveContext    
```

如果报错，则说明spark无法识别org.apache.spark.sql.hive.HiveContext，这时我们就需要采用源码编译方法得到支持hive的spark版本。

- 下载源码文件

  ​	进入官网后，可以按照下图配置选择“2.1.0(Dec 28, 2016)”和“SourceCode”，然后，在图中红色方框内，有个“Download Spark: spark-2.1.0.tgz”的下载链接，点击该链接就可以下载Spark源码文件了。



- 编译过程

  ```shell
  cd /home/hadoop/spark-2.0.2
  ./dev/make-distribution.sh —tgz —name h27hive -Pyarn -Phadoop-2.7 -Dhadoop.version=2.7.1 -Phive -Phive-thriftserver -DskipTests
  ```

  或可选择直接安装已编译好的版本，把下好的`spark-2.0.2-bin-h27hive.tgz`放到下载文件夹内

- Spark解压安装

  ```shell
  cd ~/下载                                              # 进入下载文件夹
  sudo tar -zxf spark-2.0.2-bin-h27hive.tgz -C /usr/local   # 安装至/usr/local文件夹内
  cd /usr/local                                         # 进入/usr/local文件夹
  sudo mv ./spark-1.4.0-bin-hadoop2.4/ ./spark          # 更名为spark
  sudo chown -R hadoop ./spark                          # 修改sqoop权限
  ```

- 添加环境变量

  注，如果电脑上已经装了另一个spark，此处可不增设环境变量

  ```shell
  vim ~/.profile
  ```

  添加spark安装路径

  ```shell
  #spark
  export SPARK_HOME=/usr/local/spark
  export PATH=$PATH:$SPARK_HOME/bin
  ```

  并保存修改

  ```shell
  source ~/.profile
  ```

- 修改Spark配置

  ```shell
  cd /usr/local/spark/conf                              # 进入spark配置文件夹
  sudo cp spark-env.sh.template spark-env.sh            # 复制spark-env临时文件为配置文件
  vim spark-env.sh                                      # 编辑spark配置文件
  ```

  添加下述配置信息

  ```sh
  export SPARK_DIST_CLASSPATH=$(/usr/local/hadoop/bin/hadoop classpath)
  ```

  有了上面的配置信息以后，Spark就可以把数据存储到Hadoop分布式文件系统HDFS中，也可以从HDFS中读取数据。如果没有配置上面信息，Spark就只能读写本地数据，无法读写HDFS数据。在伪分布式模式下仅测试是否安装成功时，其他配置暂时可不做修改。

- 运行样例程序

  ```shell
  cd /usr/local/spark
  bin/run-example SparkPi 2>&1 | grep "Pi is"
  ```

- 放置Hive配置文件

  为了让Spark能够访问Hive，需要把Hive的配置文件hive-site.xml拷贝到Spark的conf目录下

  ```shell
  cd /usr/local/spark/conf
  cp /usr/local/hive/conf/hive-site.xml .
  ll
  ```

- 测试是否集成成功

  ```shell
  spark-shell
  ```

  然后输入

  ```scala
  import org.apache.spark.sql.hive.HiveContext
  ```

### 2.在Hive中创建数据库和表

首先启动MySQL数据库：

```shell
service mysql start  
```

​	由于Hive是基于Hadoop的数据仓库，使用HiveQL语言撰写的查询语句，最终都会被Hive自动解析成MapReduce任务由Hadoop去具体执行，因此，需要启动Hadoop，然后再启动Hive。
然后执行以下命令启动Hadoop：

```shell
start-all.sh
```

Hadoop启动成功以后，可以再启动Hive：

```shell
hive  
```

然后在hive命令提示符内进行操作

```sql
create database if not exists sparktest;
show databases;
 create table if not exists sparktest.student(
> id int,
> name string,
> gender string,
> age int);
use sparktest;
show tables;
insert into student values(1,'Xueqian','F',23);
insert into student values(2,'Weiliang','M',24);
select * from student;
```

通过上面操作，我们就在Hive中创建了sparktest.student表，这个表有两条数据。

### 3.连接Hive读写数据

​	现在我们看如何使用Spark读写Hive中的数据。注意，操作到这里之前，你一定已经按照前面的各个操作步骤，启动了Hadoop、Hive、MySQL和spark-shell（包含Hive支持）。在进行编程之前，我们需要做一些准备工作，我们需要修改“/usr/local/sparkwithhive/conf/spark-env.sh”这个配置文件：

```shell
cd /usr/local/spark/conf/
vim spark-env.sh
```

这样就使用vim编辑器打开了spark-env.sh这个文件，输入下面内容：

```sh
export SPARK_DIST_CLASSPATH=$(/usr/local/hadoop/bin/hadoop classpath)
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export CLASSPATH=$CLASSPATH:/usr/local/hive/lib
export SCALA_HOME=/usr/local/scala
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export HIVE_CONF_DIR=/usr/local/hive/conf
export SPARK_CLASSPATH=$SPARK_CLASSPATH:/usr/local/hive/lib/mysql-connector-java-5.1.26-bin.jar
```

保存并推出，然后启动spark-shell

```shell
spark-shell
```

然后在shell界面中输入

```scala
import org.apache.spark.sql.Row
import org.apache.spark.sql.SparkSession
case class Record(key: Int, value: String)
val warehouseLocation = "spark-warehouse"
val spark = SparkSession.builder().appName("Spark Hive Example").config("spark.sql.warehouse.dir", warehouseLocation).enableHiveSupport().getOrCreate()
import spark.implicits._
import spark.sql
sql("SELECT * FROM sparktest.student").show()
```

然后再开一个命令行界面，启动hive界面，查看spark-shell中对hive表插入数据的结果

```shell
hive
```

然后输入

```sql
use sparktest;
select * from student;
```

然后在spark-shell中进行数据插入

```scala
import java.util.Properties
import org.apache.spark.sql.types._
import org.apache.spark.sql.Row
//下面我们设置两条数据表示两个学生信息
val studentRDD = spark.sparkContext.parallelize(Array("3 Rongcheng M 26","4 Guanhua M 27")).map(_.split(" "))
//下面要设置模式信息
val schema = StructType(List(StructField("id", IntegerType, true),StructField("name", StringType, true),StructField("gender", StringType, true),StructField("age", IntegerType, true)))
//下面创建Row对象，每个Row对象都是rowRDD中的一行
val rowRDD = studentRDD.map(p => Row(p(0).toInt, p(1).trim, p(2).trim, p(3).toInt))
//建立起Row对象和模式之间的对应关系，也就是把数据和模式对应起来
val studentDF = spark.createDataFrame(rowRDD, schema)
//查看studentDF
studentDF.show()
//下面注册临时表
studentDF.registerTempTable("tempTable")
sql("insert into sparktest.student select * from tempTable")
```

然后切换到hive窗口，查看数据库内容变化

```sql
select * from student;
```

能够查询到新增数据结果，则说明操作成功。

