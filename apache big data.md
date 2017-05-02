#大数据组件介绍
>技术研发中心  

#组件
----

![](http://i.imgur.com/TGjoj1b.png)

##Hadoop

> hadoop主要包含三个组件：hdfs、mapreduce、yarn


分布式存储系统HDFS （Hadoop Distributed File System ）
- 
> 分布式存储系统

> 提供了 高可靠性、高扩展性和高吞吐率的数据存储服务
![](http://i.imgur.com/cdSwV6K.png)

HDFS优点：
>高容错性
>> 数据自动保存多个副本

>> 副本丢失后，自动恢复

>
适合批处理
>> 移动计算而非数据

>> 数据位置暴露给计算框架

>适合大数据处理
>> GB 、TB 、甚至PB 级数据

>>百万规模以上的文件数量

>>10K+ 节点

>可构建在廉价机器上
>>通过多副本提高可靠性

>>提供了容错和恢复 机制
 
HDFS缺点：
>低延迟数据访问
>>比如毫秒级

>>低延迟与高吞吐率

>小文件存取
>>占用NameNode 大量内存

>>寻道时间超过读取时间

>并发写入、文件随机修改
>>一个文件只能有一个写者

>>仅支持append

分布式计算框架MapReduce
- 
> 分布式计算框架

> 具有易于编程、高容错性和高扩展性等优点
![](http://i.imgur.com/Yu1WncF.png)

YARN
-
>Hadoop 2.0新引入的资源管理系统，直接从MRv1演化而来的；

![](http://i.imgur.com/THygjuC.png)

>>核心思想：将MRv1中JobTracker的资源管理和任务调度两个功能分开，分别由ResourceManager和ApplicationMaster进程实现。

>>ResourceManager：负责整个集群的资源管理和调度

>>ApplicationMaster：负责应用程序相关的事务，比如任务调度、任务监控和容错等

>YARN的引入，使得多个计算框架可运行在一个集群中

>>每个应用程序对应一个ApplicationMaster

>>目前多个计算框架可以运行在YARN上，比如MapReduce、Spark、Storm等

##Hbase

>HBase – Hadoop Database，是一个高可靠性、高性能、面向列、可伸缩、实时读写的分布式数据库

>利用Hadoop HDFS作为其文件存储系统,利用Hadoop MapReduce来处理HBase中的海量数据,利用Zookeeper作为其分布式协同服务

>主要用来存储非结构化和半结构化的松散数据（列存 NoSQL 数据库）

> HBase数据模型
> 
![](http://i.imgur.com/76Y8t0h.png)

> Row Key决定一行数据,只能存储64k的字节数据
> 
> Timestamp时间戳在HBase每个cell存储单元对同一份数据有多个版本，根据唯一的时间戳来区分每个版本之间的差异，不同版本的数据按照时间倒序排序，最新的数据版本排在最前面
>  
>  Column Family (CF)列族HBase表中的每个列都归属于某个列族，列族必须作为表模式(schema)定义的一部分预先给出。列名以列族作为前缀，每个“列族”都可以有多个列成员(column)；如CF1:math, CF2:english, 新的列族成员（列）可以随后按需、动态加入

##Hive

>Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行

>结构

![](http://i.imgur.com/3QsE8Sw.png)

>Hive将元数据存储在关系型数据库中。Hive中的元数据包括表的名字，表的列和分区及其属性，表的属性（是否为外部表等），表的数据所在目录等。

>Hive的数据存储在HDFS中，大部分的查询、计算由MapReduce完成（包含*的查询，比如select * from tbl不会生成MapRedcue任务）。

##Kylin

>Kylin一个开源的分布式分析引擎，提供Hadoop之上的SQL查询接口及多维分析（OLAP）能力以支持超大规模数据

![](http://i.imgur.com/oKxHeoG.png)

>Kylin的核心思想是预计算，即对多维分析可能用到的度量进行预计算，将计算好的结果保存成Cube，供查询时直接访问。把高复杂度的聚合运算、多表连接等操作转换成对预计算结果的查询，这决定了Kylin能够拥有很好的快速查询和高并发能力。
![](http://i.imgur.com/BCCyttP.png)

>假设我们有4个dimension，这个Cube中每个节点（称作Cuboid）都是这4个dimension的不同组合，每个组合定义了一组分析的dimension（如group by），measure的聚合结果就保存在这每个Cuboid上。查询时根据SQL找到对应的Cuboid，读取measure的值，即可返回。
>![](http://i.imgur.com/InfEvAB.png)

##Spark

>Spark是专为大规模数据处理而设计的快速通用的计算引擎,启用了内存分布数据集

![](http://i.imgur.com/df5XVE3.png)

>Spark在架构上包括Spark Core和4个官方子模块--Spark SQL、Spark Streaming、机器学习库MLlib和图计算库GraphX，Spark的各个子模块以Spark Core为基础，进一步支持更多的计算场景。Spark专注于数据的计算，而数据的存储在生产环境中往往还是由Hadoop分布式文件系统HDFS承担。

>Spark中最重要的概念：RDD（Resilient Distributed Dataset———弹性分布式数据集），RDD是Spark中对数据和计算的抽象，是Spark中最核心的概念，它表示已被分片（partition），不可变的并能够被并行操作的数据集合。对RDD的操作分为两种transformation和action。Transformation操作是通过转换从一个或多个RDD生成新的RDD。Action操作是从RDD生成最后的计算结果。

![](http://i.imgur.com/0pzAZM1.png)

>Spark的计算发生在RDD的action操作，而对action之前的所有transformation，Spark只是记录下RDD生成的轨迹，而不会触发真正的计算。
Spark内核会在需要计算发生的时刻绘制一张关于计算路径的有向无环图，也就是DAG。

##Sqoop
>用于在Hadoop(Hive)与传统的数据库(mysql、postgresql...)间进行数据的传递，可以将一个关系型数据库（例如 ： MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。

![](http://i.imgur.com/3Q3JAI6.png)


>1.Sqoop会通过JDBC来获取所需要的数据库元数据，例如，导入表的列名，数据类型等。

>2.这些数据库的数据类型(varchar, number等)会被映射成Java的数据类型(String, int等)，根据这些信息，Sqoop会生成一个与表名同名的类用来完成反序列化工作，保存表中的每一行记录。

>3.Sqoop启动MapReducer作业

>4.启动的作业在input的过程中，会通过JDBC读取数据表中的内容，这时，会使用Sqoop生成的类进行反序列化操作

>5.将这些记录写到HDFS中，在写入到HDFS的过程中，同样会使用Sqoop生成的类进行序列化

##Flume
>Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。

![](http://i.imgur.com/0LAy4l6.png)

>多agent配置

![](http://i.imgur.com/NZ6RV5E.png)

>多路扇出数据流

![](http://i.imgur.com/TJp9lwm.png)

##Kafka
>Kafka是一种高吞吐量的分布式发布消息队列，处理动作流数据。

![](http://i.imgur.com/06PfpwT.png)
>Broker:Kafka集群包含一个或多个服务器，这种服务器被称为broker

>Topic:每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic。物理上不同Topic的消息分开存储，逻辑上一个Topic的消息虽然保存于一个或多个broker上但用户只需指定消息的Topic即可生产或消费数据而不必关心数据存于何处

>Partition:是物理上的概念，每个Topic包含一个或多个Partition

>Producer:负责发布消息到Kafka broker

>Consumer:消息消费者，向Kafka broker读取消息的客户端。

>Consumer Group:每个Consumer属于一个特定的Consumer Group可为每个Consumer指定group name，若不指定group name则属于默认的group。

#应用场景

##日志收集处理
![](http://i.imgur.com/bcslRpZ.png)

>用户的线上行为会被记录为用户日志，flume监控日志文件目录，配置扇出通道，其一把数据同步到hdfs，其二把数据写到kafaka集群。hdfs上的数据应用hive进行分析，并把分析结果数据通过sqoop导入到mysql；kafka中的数据则通过spark streaming进行流式处理，结果数据存入redis。

##新平台项目应用
![](http://i.imgur.com/VwWJFzL.png)

>该项目通过hive按照日期分区的方式，每天定时把新增数据导入到新增分区中进行分析，kylin使用hive的KYLIN_ANALYSIS表，把全表的字段作为维度，对表中的:USER_ID，IP，URL,TP(页面停留时间)，进行度量，通过kylin的预计算，把结果存放到hbase中，用来提升查询效率。