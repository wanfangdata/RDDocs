大数据组件介绍
-
>技术研发中心  

组件
-

![](http://i.imgur.com/TGjoj1b.png)

Hadoop
-
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

Hbase
-
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

Hive
-
>Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行

>结构

![](http://i.imgur.com/3QsE8Sw.png)

>Hive将元数据存储在关系型数据库中。Hive中的元数据包括表的名字，表的列和分区及其属性，表的属性（是否为外部表等），表的数据所在目录等。

>Hive的数据存储在HDFS中，大部分的查询、计算由MapReduce完成（包含*的查询，比如select * from tbl不会生成MapRedcue任务）。

Kylin
-
>Kylin一个开源的分布式分析引擎，提供Hadoop之上的SQL查询接口及多维分析（OLAP）能力以支持超大规模数据

![](http://i.imgur.com/oKxHeoG.png)

>Kylin的核心思想是预计算，即对多维分析可能用到的度量进行预计算，将计算好的结果保存成Cube，供查询时直接访问。把高复杂度的聚合运算、多表连接等操作转换成对预计算结果的查询，这决定了Kylin能够拥有很好的快速查询和高并发能力。
![](http://i.imgur.com/BCCyttP.png)

>假设我们有4个dimension，这个Cube中每个节点（称作Cuboid）都是这4个dimension的不同组合，每个组合定义了一组分析的dimension（如group by），measure的聚合结果就保存在这每个Cuboid上。查询时根据SQL找到对应的Cuboid，读取measure的值，即可返回。
>![](http://i.imgur.com/InfEvAB.png)

Spark
-
>Spark是专为大规模数据处理而设计的快速通用的计算引擎,启用了内存分布数据集

![](http://i.imgur.com/df5XVE3.png)

>Spark在架构上包括Spark Core和4个官方子模块--Spark SQL、Spark Streaming、机器学习库MLlib和图计算库GraphX，Spark的各个子模块以Spark Core为基础，进一步支持更多的计算场景。Spark专注于数据的计算，而数据的存储在生产环境中往往还是由Hadoop分布式文件系统HDFS承担。

>Spark中最重要的概念：RDD（Resilient Distributed Dataset———弹性分布式数据集），RDD是Spark中对数据和计算的抽象，是Spark中最核心的概念，它表示已被分片（partition），不可变的并能够被并行操作的数据集合。对RDD的操作分为两种transformation和action。Transformation操作是通过转换从一个或多个RDD生成新的RDD。Action操作是从RDD生成最后的计算结果。

![](http://i.imgur.com/0pzAZM1.png)

>Spark的计算发生在RDD的action操作，而对action之前的所有transformation，Spark只是记录下RDD生成的轨迹，而不会触发真正的计算。
Spark内核会在需要计算发生的时刻绘制一张关于计算路径的有向无环图，也就是DAG。

Sqoop
-
>用于在Hadoop(Hive)与传统的数据库(mysql、postgresql...)间进行数据的传递，可以将一个关系型数据库（例如 ： MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。

![](http://i.imgur.com/3Q3JAI6.png)


>1.Sqoop会通过JDBC来获取所需要的数据库元数据，例如，导入表的列名，数据类型等。

>2.这些数据库的数据类型(varchar, number等)会被映射成Java的数据类型(String, int等)，根据这些信息，Sqoop会生成一个与表名同名的类用来完成反序列化工作，保存表中的每一行记录。

>3.Sqoop启动MapReducer作业

>4.启动的作业在input的过程中，会通过JDBC读取数据表中的内容，这时，会使用Sqoop生成的类进行反序列化操作

>5.将这些记录写到HDFS中，在写入到HDFS的过程中，同样会使用Sqoop生成的类进行序列化

Flume
-
>Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。

![](http://i.imgur.com/0LAy4l6.png)

>多agent配置

![](http://i.imgur.com/NZ6RV5E.png)

>多路扇出数据流

![](http://i.imgur.com/TJp9lwm.png)

Kafka
-
>Kafka是一种高吞吐量的分布式发布消息队列，处理动作流数据。

![](http://i.imgur.com/06PfpwT.png)
>Broker:Kafka集群包含一个或多个服务器，这种服务器被称为broker

>Topic:每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic。物理上不同Topic的消息分开存储，逻辑上一个Topic的消息虽然保存于一个或多个broker上但用户只需指定消息的Topic即可生产或消费数据而不必关心数据存于何处

>Partition:是物理上的概念，每个Topic包含一个或多个Partition

>Producer:负责发布消息到Kafka broker

>Consumer:消息消费者，向Kafka broker读取消息的客户端。

>Consumer Group:每个Consumer属于一个特定的Consumer Group可为每个Consumer指定group name，若不指定group name则属于默认的group。

应用场景
-

日志收集处理
-
![](http://i.imgur.com/bcslRpZ.png)

> 1.用户的线上行为记录
>>用户的访问nginx的行为，会被自定义的日志类，记录为特定格式的日志例如（user^ip^date^url）并保存到指定的日志目录下。

> 2.flume收集日志
>>flume监控指定的日志目录，根据定义好的的sink、source（这里用到了多路扇出数据流，分别流向hadoop和kafka）。

> 3.数据处理
>>3.1.离线部分
>>>流向hadoop的数据会被存储到hdfs中，并根据hadoop的配置的副本数，自动的做数据复制，保证数据的高可用性。

>>>通过hive创建与日志格式相对应的hive外表（防止删表时删除数据），然后load数据，把数据真实存在的目录映射到hive表中，使用hive进行分析。

>>>hive的分析结果存在于hive表中，通过sqoop导出到mysql中以便web展示使用。

>>3.2.实时部分
>>>流向kafka的数据被分散到集群中的多个kafka服务器上减轻单个服务器的压力，并根据配置的时间策略进行删除。

>>>spark streaming作为消费者去消费kafka中的数据，并把实时处理（秒级延迟）的结果数据写回到kafka或者redis,以便其他程序去调用。
新平台项目应用
-
![](http://i.imgur.com/jt5TIgY.png)

>1.用户线上行为记录
>>用户访问http://new.wanfangdata.com.cn/时会根据定义的日志类，记录用户的行为。包括：用户操作行为（user\_operate）、用户注册行为（user_register）。每隔一个小时在对应的目录下生成一个文件名称为xxx.log.yyyy-mm-dd-hh

>2.flume收集日志
>>由于要监控两个不同的日志目录，并导出到hdfs不同的目录下，所以flume采用多数据源，多扇出通道的方式进行配置。

>>source1监控user\_operate日志目录，并把数据通过扇出通道sink1写入hdfs上对应的 /flume/wanfang/user\_operate_log/YEAR/MONTH/DAY/HOUR目录下。

>>source2监控user_register日志目录，并把数据通过扇出通道sink2写入hdfs上对应的 /flume/wanfang/user\_register\_log/YEAR/MONTH/DAY/HOUR目录下。

>3.数据处理
>>3.1.hive分析

>>>根据两种日志的格式，在hive中创建与日志格式对应的hive外表：user\_operate\_log表、user\_registered_log表。这两张表都是以:年、月、日、时进行分区的。linux定时任务会把hdfs上每天每小时新增的日志映射到这两张表的对应分区中，用来实现对新增数据的处理。

>>>对user\_operate\_log表进行与业务相关的组合查询，hive会把执行的hql语句转化为mapreduce任务提交到hadoop集群中进行计算，得出不同的分析指标：基础指标、访客属性、搜索引擎分析、网站浏览量、独立访客数、检索词、全部来源、外部链接、资源使用、网站概况等指标。这些指标会从 小时维度 和 天维度 分别进行统计。

>>>对user\_registered_log表进行与业务相关的组合查询，hive会把执行的hql语句转化为mapreduce任务提交到hadoop集群中进行计算，得出不同的分析指标：新增访客数、活跃率等指标。这些指标会从 小时维度 和 天维度 分别进行统计。

>>>以上的分析结果会通过sqoop导出到mysql集群中，以便web展示使用。

>>3.2.kylin分析
>>>kylin以hive中的kylin\_analysis表为数据源，该表按照：日、时进行分区，定时把新增数据导入分区中用来处理新增的数据。

>>>提取user\_operate\_log表中user_id、ip、age、education\_leve、reserch\_domain、title等字段，插入到kylin\_analysis表中。

>>>kylin ui（http://10.10.184.215:7070/kylin）上新建kylin model、kylin cube。
>
>>>这里使用数据源表kylin\_analysis作为维度表和查询表。以表中的所有字段作为维度建立kylin数据模型。在该数据模型的基础上创建包含所有维度的cube，对表中的:user_id、ip、url、tp(页面停留时间)，进行预计算，把计算结果存入hbase中，用来提升查询效率。

>>>web展示可以通过java api进行查询，也可以使用restful api进行查询。
