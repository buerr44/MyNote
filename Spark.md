# SparkCore

## 1. Spark概述

### 1.1 Spark是什么

Spark是一种基于内存的快速, 通用, 可扩展的大数据分析计算引擎;

### 1.2 Spark and Hadoop

功能上来看:

- **Hadoop**
  - 由java语言编写, 在分布式服务器集群上存储海量数据并运行分布式分析应用的开源框架;
  - 作为Hadoop分布式文件系统, HDFS出于Hadoop生态圈的最下层, 存储着所有的数据, 支持着Hadoop的所有服务;
  - MapReduce是一种编程模型, 在处理海量数据时, 性能横向扩张变得非常容易;
  - HBase是对Google的Bigtable的开源实现, 是一个基于HDFS的分布式数据库, 商场实时地随机读/写超大规模数据集;
- **Spark**
  - 由Scala语言开发的快速, 通用, 可扩展的大数据分析引擎;
  - Spark Core提供了Spark最基础与最核心的功能
  - Spark SQL是Spark用来操作结构化数据的组件, 通过Spark SQL, 用户可以使用SQL或者HQL来查询数据;
  - Spark Streaming是Spark平台上针对实时数据进行流式计算的组件, 提供了丰富的处理数据流的API

总而言之, Spark出现的时间相对较晚, 并且主要功能主要是用于数据计算, 所以被认为是Hadoop框架的升级版;

### 1.3 Spark or Hadoop

- Hadoop MR由于涉及初衷并不是为了满足循环迭代式数据流处理, 因此在多并行的数据可复用场景(如:机器学习,图挖掘算法等)中存在诸多计算效率等问题, 所以Spark应运而生. Spark就是在传统的MR计算框架基础上, 利用计算过程的优化, 加快了数据分析,挖掘的运行和读写速度, 并将计算单元缩小到更适合并行计算和重复使用的RDD计算模型;
- 机器学习中ALS, 凸优化梯度下降等, 都需要基于数据集或数据集的衍生数据反复查询反复操作, MR这种模式不太合适;
- Spark是一个分布式数据快速分析项目, 核心技术是弹性分布式数据集, 提供了比MR丰富的模型, 可以快速在内存中对数据集进行多次迭代, 来支持负责的数据挖掘算法和图形计算算法;
- **Spark和Hadoop的根本差异是多个作业之间的数据通信问题: Spark多个作业之间数据通信是基于内存, 而Hadoop是基于磁盘**;
- Spark Task的启动时间快, Spark采用fork线程的方式, 而Hadoop采用创建新的进程的方式;
- Spark只有在shuffler的时候将数据写入磁盘, 而Hadoop中多个MR作业之间的数据交互都要依赖于磁盘交互;
- Spark的缓存机制比HDFS的缓存机制高效;

绝大多数的数据计算场景中, Spark确实会比MR更有优势, 但是Spark是基于内存的, 所以在实际生产环境中, 由于内存的限制, 可能会由于内存资源不够导致Job执行失败, 所以MR其实是一个更好的选择, Spark并不能完全替代MR;

### 1.4 Spark核心模块

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415164853666.png" alt="image-20210415164853666" style="zoom:67%;" />

- **Spark Core**

  提供了Spark最基础和最核心的功能, Spark其他的工嗯呢都是在Spark Core的基础上进行扩展的;

- **Spark SQL**

  是Spark用来操作结构化数据的组件,  通过Spark SQL, 用户可以使用SQL或DQL来查询数据

- **Spark Streaming**

  是Spark平台上针对实时数据进行流式计算的组件, 提供了丰富的处理数据流的API

- **Spark MLlib**

  MLlib是Spark提供的一个机器学习算法库, 不仅提供了模型评估,数据导入等额外功能,还提供了一些更底层的机器学习原语;

- **Spark GraphX**

  GraphX是Spark面向图计算提供的框架与算法库;

## 2. Spark快速上手实例

详见<尚硅谷大数据技术之SparkCore> 第**6-9**页

## 3. Spark运行环境

在国内工作中主流的环境为Yarn, 不过逐渐容器式环境也慢慢流行起来

### 3.1 Local模式

不需要其他任何节点资源就可以在本地执行Spark代码的环境, 一般用于教学,调试,演示等;

详细配置见SparkCore第**10-11**页

### 3.2 Standalone模式

独立部署模式, 体现了经典的master-slave模式;

由Spark自身提供计算资源, 无需其他框架提供资源, 降低了和其他第三方资源框架的耦合性, 独立性非常强;

详细配置见SparkCore第**12-18**页

### 3.3 Yarn模式

Spark主要是计算框架, 而不是资源调度框架, 所以本身提供的资源调度不是它的强项, 所以还是和其他专业资源调度框架集成会更靠谱些;

详细配置见SparkCore第**18-21**页

### 3.4 K8S & Mesos模式

Mesos是Apache下的开源分布式资源管理框架, 它被称为分布式系统的内核;

**容器化部署**是目前业界流行的一项技术, 基于Docker镜像运行能够让用户更加方便地对应用进行管理和运维, 容器管理工具中国最为流行的就是kubernetes(k8s);

### 3.5 Windows模式

可以在不使用虚拟机的情况下, 也能学习Spark的基本使用;

- 解压缩文件: 将文件spark-3.0.0-bin-hadoop3.2.tgz解压缩到无中文空格的路径中;

- 启动本地环境

  - 执行解压缩文件路径下bin目录中的spark-shell.cmd文件, 启动Spark本地环境;
  - 在bin目录中创建input目录, 并添加word.txt文件, 在命令行中输入脚本代码;

- 命令行提交应用

  在DOS命令行窗口汇中执行提交指令

  <img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210419101029691.png" alt="image-20210419101029691" style="zoom:80%;" />

### 3.6 部署模式对比

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210419101107805.png" alt="image-20210419101107805" style="zoom:67%;" />

### 3.7 端口号

- Spark查看当前Spark-shell运行任务情况端口号: 4040(计算)
- Spark Master内部通信服务端口号: 7077
- Standalone模式下, Spark Master Web端口号: 8080(资源)
- Spark历史服务器端口号:18080
- Hadoop Yarn任务运行情况查看端口号: 8088

## 4. Spark运行架构

### 4.1 运行架构

Spar框架的核心是一个计算引擎, 整体来说, 它采用了标准master-slave的结构;

下图是Spark执行时的基本结构, 图形中的Driver表示master, 负责管理整个集群中的作业任务调度, 图形中的Executor则是slave, 负责实际执行任务:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415170214732.png" alt="image-20210415170214732" style="zoom: 80%;" />

### 4.2 核心组件

#### Driver

Spark驱动器节点,  用于Spark任务中的main方法, 负责实际代码的执行工作;

Driver在Spark作业执行时主要负责:

- 将用户程序转化为作业(job);
- 在Executor之间调度任务(task);
- 跟踪Executor的执行情况;
- 通过UI展示查询运行情况;

所谓的Driver就是驱使整个应用运行起来的程序, 也称之为Driver类;

#### Executor

Executor是集群中工作节点(Worker)中的一个JVM进程, 负责在Spark作业中运行具体任务(Task), 任务彼此之间相互独立. Spark应用启动时, Executor节点被同时启动, 并且四种伴随着整个Spark应用的生命周期而存在;

**两大核心功能:**

- 负责运行组成Spark应用的任务, 并将结果返回给驱动器进程;
- 通过自身的块管理器(Block Manager)为用户程序中要求缓存的RDD提供内存式存储; RDD是直接缓存在Executor进程内的, 因此任务可以在运行时充分利用缓存数据加速运算;

#### Master & Worker

Spark集群的独立部署环境中, 不需要依赖其他的资源调度框架, 自身就实现了资源调度功能, 所以还有其他两个核心组件: Master和Worker;

- Master是一个进程, 主要负责资源的调度和分配, 并进行集群的监控等职责, 类似于Yarn环境中的RM;
- Worker也是个进程, 一个Worker运行在集群中的一台服务器上, 由Master分配资源对数据进行并行的处理和计算, 类似Yarn环境的NM;

#### ApplicationMaster

用于向资源调度器申请执行任务的资源容器(Container), 运行用户自己的程序任务job, 监控整个任务的执行, 跟踪整个任务的状态, 处理任务失败等异常情况

### 4.3 核心概念

#### Executor与Core

Spark Executor是集群汇总运行在工作节点Worker中的一个JVM进程, 是整个集群中的专门用于计算的节点; 在提交应用中, 可以提供参数指定计算节点的个数, 以及对应的资源. 资源一般指的是工作节点Executor的内存大小和使用的虚拟CPU核(Core)数量;

#### 并行度(Parallelism)

整个集群并行执行任务的数量称之为并行度; 一个作业的并行度取决于框架的默认配置, 应用程序也可以在运行过程中动态修改;

#### 有向无环图(DAG)

由Spark程序直接映射层的数据流的高级抽象模型, 简单理解就是将整个程序计算的执行过程用图形表示出来, 更直观便于理解, 用于表示程序的拓扑结构; DAG具有方向, 不会闭环

### 4.4 提交流程

国内将Spark引用部署到Yarn环境中会多一点, Spark提交到Yarn环境中执行的时候, 一般会有两种部署执行方式: Client和Cluster.  **两种模式的区别在于: Driver程序的运行节点位置**;

#### Yarn Client模式

Client模式将用于监控和调度的Driver模块在客户端执行, 而不是在Yarn中, 所以一般用于测试

1. Driver在任务提交的本地机器上运行
2. Driver启动后悔和RM通讯申请启动ApplicationMaster
3. RM分配Container, 在合适的NM上启动ApplicationMaster, 负责向RM申请Executor内存;
4. RM接到ApplicationMaster的资源申请会分配Container, 然后ApplicationMaster在资源分配制定的NM上启动Executor进程;
5. Executor进程启动后会向Driver反向注册, Executor全部注册完成后Driver开始执行main函数
6. 之后执行到Action算子时, 触发一个Job, 并根据宽依赖开始划分stage, 每个stage生成对应的TaskSet, 之后将task分发到各个Executor上执行;

#### Yarn Cluster模式

Cluster模式将用于监控和调度的Driver模块启动在Yarn集群资源中执行, 一般应用于实际生产环境

1. 在Yarn Cluster模式下, 任务提交后会和RM通讯申请启动ApplicationMaster;
2. RM分配Container, 在合适的NM上启动ApplicationMaster, 此时的ApplicationMaster就是Driver
3. Driver启动后向RM申请Executor内存, RM接到ApplicationMaster的资源申请后悔分配container, 然后在合适的NM上启动Executor进程;
4. Executor进程启动后会向Driver反向注册, Executor全部注册完成后Driver开始执行main函数;
5. 之后执行到Action算子时, 触发一个Job, 并根据宽依赖开始划分stage, 每个stage生成对应的TaskSet, 之后将task分发到各个Executor上执行;

## 5. Spark核心编程

Spark计算框架为了能够进行高并发和高吞吐的数据处理, 封装了三大数据结构, 用于处理不同的应用场景, 分别是:

- RDD: 弹性分布式数据集;
- 累加器: 分布式共享**只写**变量;
- 广播变量: 分布式共享**只读**变量;

### 5.1 RDD

RDD(Resilient Distributed Dataset)叫做弹性分布式数据集, 是Spark中**最基本的数据处理模型**; 代码中是一个抽象类, 代表一个弹性的, 不可变, 可分区, 里面的元素可并行计算的集合;

- 弹性:
  - 存储的弹性: 内存与磁盘的自动切换;
  - 容错的弹性: 数据丢失可以自动恢复;
  - 计算的弹性: 计算出错重试机制;
  - 分片的弹性: 可根据需要重新分片;
- 分布式: 数据存储在大数据集群不同节点上;
- 数据集: RDD封装了计算逻辑, 并不保存数据;
- 数据抽象: RDD是一个抽象类, 需要子类具体实现;
- 不可变: RDD封装了计算逻辑, 是不可以改变的, 想要改变只能产生新的RDD;
- 可分区, 并行计算

#### RDD执行原理

1. 启动Yarn集群环境
2. Spark通过申请资源创建调度节点和计算节点;
3. Sprak框架根据需求将计算逻辑根据分区划分成不同的任务;
4. 调度节点将任务根据计算节点状态发送到对应的计算节点进行计算;

RDD在整个流程中主要用于将逻辑进行封装, 并生成Task发送给Executor节点执行计算;

#### 基础编程

详见SpringCore第**33-62**页

### 5.2 累加器

#### 实现原理

累加器用来把Executor端变量信息聚合到Driver端; 在Driver程序中定义的变量, 在Executor端的每个Task都会得到这个变量的一份新的副本, 每个task更新这些副本的值后, 传回Driver端进行merge;

#### 基础编程

详见SpringCore第**63-64**页

### 5.3 广播变量

#### 实现原理

广播变量用来高效分发较大的对象, 向所有工作节点发送一个较大的只读值, 以供一个或多个Spark操作使用;

## 6. Spark案例实操

详见SpringCore第**65-68**页

# SparkSQL

## 1 SparkSQL概述

### 1.1 SparkSQL是什么

SparkSQL是Spark用于结构化数据处理的Spark模块;

### 1.2 Hive and SparkSQL

SparkSQL摆脱了对Hive的依赖性, 无论在数据兼容, 性能优化, 组件扩展方面都得到了极大方便

- 数据兼容方面, SparkSQL补单兼容Hive, 还可以从RDD, parquet文件, JSON文件中获取数据, 未来版本甚至支持获取RDBMS数据以及cassandra等NOSQL数据;
- 性能优化方面, 除了采取In-Memory Columnar Storage, byte-code generation等优化技术外, 将会引进Cost Model对查询进行动态评估, 获取最佳物理计划等等;
- 组件扩展方面, 无论是SQL的语法解析器,分析器还是优化器都可以重新定义, 进行扩展;

SparkSQL可以简化RDD的开发, 提高开发效率, 且执行效率非常快, 所以实际工作中基本上采用的是SqarkSQL;

SparkSQL为了简化RDD的开发, 提高开发效率, 提供了2个编程抽象, 类似Spark Core中的RDD:

- DataFrame
- DataSet

### 1.3 SparkSQL特点

- **易整合**: 无缝的整合了SQL查询和Spark编程;
- **统一的数据访问**: 使用相同的方式连接不同的数据源
- **兼容Hive**: 在已有的仓库上直接运行SQL或者HiveQL
- **标准数据连接**: 通过JDBC或者ODBC来连接;

### 1.4 DataFrame

- 在Spark中, DataFrame是一种以RDD为基础的分布式数据集, 类似于传统数据库中的二维表格;
- DataFrame与RDD的主要区别在于, DataFrame带有schema元信息, 即DataFrame所表示的二维表数据集的每一列都带有名称和类型; 而RDD无从得知所存数据元素的具体内部结构, 导致Spark Core只能在stage层面进行简单通用的流水线优化;
- DataFrame思维数据提供了schema的视图, 可以把它当做数据库中的一张表来对待;
- DataFrame是懒执行, 但性能上比RDD高, 主要原因: 优化的执行计划, 即查询计划通过Spark catalyst optimiser进行优化;

### 1.5 DataSet

DataSet是分布式数据集合, 提供了RDD的优势(强类型,使用强大的lambda函数的能力)以及SparkSQL优化执行引擎的优点;

- DataSet是DataFrame API的一个扩展, 是SparkSQL最新的数据抽象;
- 用户友好的API风格, 既具有类型安全检查也具有DataFrame的查询优化特性;
- 用样例类来对DataSet中定义数据的结构信息, 样例类中每个属性的名称直接映射到DataSet中的字段名称;
- DataSet是强类型的, 比如可以有DataSet[Car], DataSet[Person];
- **DataFrame是DataSet的特列**, DataFrame = DataSet[Row], 所以可通过as方法将DataFrame转换为DataSet;

## 2. SparkSQL核心编程

### 2.1 新的起点

SparkCore中, 如果想要执行应用程序, 需要首先构建上下文环境对象SparkContext;

SparkSQL可以理解为对Spark Core的一种封装, 不仅仅在模型上进行了封装, 上下文环境对象也进行了封装;

SparkSession是Spark最新的SQL查询起始点, 实质上是SQLContext和HiveContext的组合; 其内部封装了SparkContext, 计算实际上是由SparkContext完成的;

### 2.2 DataFrame

详见SpringSQL第**8-12**页

### 2.3 DataSet

详见SpringSQL第**12-13**页

### 2.4 RDD, DataFrame, DataSet三者的关系

#### 三者的共性

- RDD, DataFrame, DataSet全都是spark平台下的分布式弹性数据集, 为处理超大型数据提供便利
- 三者都有惰性机制, 在进行创建, 转换方法时, 不会立即执行, 只有遇到Action如foreach时, 三者才会开始遍历运算;
- 三者有许多共同的函数, 如filter, 排序等;
- 在DataFrame和DataSet的许多操作都需要这个包: import spark.implicits_
- 三者都会根据Spark的内存情况自动缓存运算, 这样即使数据量很大,也不用担心会内存溢出
- 三者都有Partition概念

#### 三者的区别

1. RDD:
   - RDD一般和spark mllib同时使用
   - RDD不支持sparksql操作
2. DataFrame
   - 与RDD和DataSet不同, DataFrame每一行的类型固定位Row, 每一列的值没法直接访问, 只有通过解析才能拿获取各个字段的值;
   - DataFrame与DataSet一般不与spark mllib同时使用
   - DataFrame与DataSet均支持SparkSQL的操作, 还能注册临时表/视窗, 进行sql语句操作
   - DataFrame与DataSet支持一些特别方便的保存方式, 比如保存成csv;
3. DataSet
   - DataSet和DataFrame拥有完全相同的成员函数, 区别只是每一行的数据类型不同, DataFrame其实就是DataSet的一个特例;
   - DataFrame亦可以叫DataSet[Row], 每一行类型时Row; 而DataSet每一行的类型是不一定的, 在自定义了case class之后可以自由获得每一行的信息

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210416102857812.png" alt="image-20210416102857812" style="zoom:67%;" />

### 2.5 IDEA开发SparkSQL

详见SparkSQL第**16-17**页;

### 2.6 用户自定义函数

详见SparkSQL第**17-21**页;

### 2.7 数据的加载和保存

详见SparkSQL第**21-27**页;

## 3. SparkSQL项目实战

详见SparkSQL第**28-29**页;

# SparkStreaming

## 1. SparkStreaming概述

### 1.1 Spark Streaming是什么

Spark Streaming用于流式数据的处理, 支持的数据输入源很多, 例如:Kafka,Flume,Twitter,ZeroMQ和简单的TCP套接字等等, 数据输入后可以用Spark的高度抽象原语如: map,reduce,join,window等进行运算, 而结果也能保存在很多地方, 如HDFS,数据库等;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210419091708128.png" alt="image-20210419091708128" style="zoom:67%;" />

和Spark基于RDD的概念相似, Spark Streaming使用离散化流作为抽象表示, 叫做DStream. DStream是随时间推移而受到的数据序列, 简单来说, DStream就是对RDD在实时数据处理场景的一种封装;

### 1.2 Spark Streaming特点

- **易用**
- **容错**
- **易整合到Spark体系**

### 1.3 Spark Streaming架构

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210419093116861.png" alt="image-20210419093116861" style="zoom:67%;" />

**背压机制:** 根据JobScheduler反馈作业的执行信息来动态调整Receiver数据接受率;

## 2. DStream入门

详见SpringStreaming第**5-6**页

## 3. DStream创建

### 3.1 RDD队列

测试过程汇总, 可以通过使用ssc.queueStream(queueOfRDDs)来创建DStream, 每一个推送到这个队列中的RDD, 都会作为一个DStream来处理

案例实操详见SpringStreaming第**7-8**页

### 3.2 自定义数据源

需要继承Receiver, 并实现onStart, onStop方法来自定义数据源采集

案例实操详见SpringStreaming第**8-10**页

### 3.3 Kafka数据源(面试,开发重点)

#### 版本选型

- **ReceiverAPI**: 需要一个专门的Executor去接收数据, 然后发送给其他的Executor做计算. 存在问题若果接收数据的Executor速度大于计算的Executor速度, 会导致计算数据的节点内存溢出. 当前版本不适用;
- **DirectAPI**: 是由计算的Executor来主动消费Kafka的数据, 速度由自身控制;

案例实操详见SpringStreaming第**10-14**页

## 4.DStream转换

DStream上的操作与RDD的类似, 分为Transformations(转换)和Output Operations(输出)两种

### 4.1 无状态转化操作

无状态转化操作就是把简单的RDD转化操作应用到每个批次上, 也就是转化DStrteam中的每一个RDD;

#### Transform

Transform允许DStream上执行任意的RDD-to-RDD函数, 通过该函数可以方便的扩展Spark API, 该函数每一批次调度一次, 其实也就是对DStream中的RDD应用转换

#### Join

两个流之间的join需要每个流的批次大小一致, 才能做到同时出发计算, 计算过程就是对当前批次的两个流中各自的RDD进行join;

### 4.2 有状态转化操作

#### UpdateStateByKey

提供了对一个状态变量的访问, 用于键值对形式的DStream,  结果时一个新的DStream, 其内部的RDD序列是由每个时间区间对应的(键,状态)对组成

UpdateStateByKey操作使得我们可以在用新信息进行更新时保持任意的状态, 分以下两步:

1. 定义状态, 状态可以是一个任意的数据类型;
2. 定义状态更新函数, 用次函数阐明如何使用之前的状态和来自输入流的新值对状态进行更新

案例实操详见SpringStreaming第**17-18**页

#### WindowOperations

WindowOperations可以设置窗口的大小和滑动窗口的间隔来动态的获取当前Streaming的允许状态, 所有基于窗口的操作都需要两个参数:

- 窗口时长: 计算内容的时间范围;
- 滑动步长: 隔多久触发一次计算;

案例实操详见SpringStreaming第**19-20**页

## 5. DStream输出

输出操作指定了对流数据经转化操作得到的数据所要执行的操作, 输出操作如下:

- print(): 在运行流程序的驱动结点上打印DStream中每一批次数据的最开始10个元素, 这用于开发和调试;
- saveAsTextFiles(prefix,[suffix]): 以text文件形式存储这个DStream内容, 每一批次的存储文件名基于参数中的prefix和suffix;
- saveAsObjectFiles(prefix,[suffix]): 以Java对象序列化的方式将Stream中的数据保存为SequenceFiles;
- saveAsHadoopFiles(prefix,[suffix]): 将Stream中的数据保存为Hadoop Files;
- foreachRDD(func): 这是最通用的输出操作, 即将函数func用于产生于stream的每一个RDD, 其中参数传入的函数func应该实现将每一个RDD中数据推送到外部系统;

通用的输出操作foreachRDD(), 它用来对DStream中的RDD运行任意计算, 可以重用我们在Spark中实现的所有行动操作, 常见的用例之一是把数据写到诸如MySQL的外部数据库中;

1. 连接不能写在driver层面(序列化);
2. 如果写在foreach则每个RDD中的每一条数据都创建, 得不偿失
3. 增加foreachPartition, 在分区创建(获取)

## 6. 优雅关闭

流式无需要7*24小时执行, 但是有时涉及到升级代码需要主动停止程序, 但是分布式程序没法一个个进程去杀死. 所有配置优雅地关闭就显得至关重要了, 使用外部文件系统来控制内部程序关闭

- MonitorStop
- SparkTest

案例实操详见SpringStreaming第**22-23**页

## 7. SparkStreaming案例实操

案例实操详见SpringStreaming第**24-27**页

# Spark内核

## 1. Spark内核概述

Spark内核泛指Spark的核心运行机制, 包括Spark核心组件的运行机制,Spark任务调度机制,Spark内存管理机制,Spark核心功能的运行原理等;

### 1.1 核心组件回顾

#### Driver

Spark驱动器节点, 用于执行Spark任务中的main方法, 负责实际代码的执行工作, 主要负责

1. 将用户程序转化为作业(Job)
2. 在Executor之间调度任务(Task)
3. 跟踪Executor的执行情况;
4. 通过UI展示查询运行情况;

#### Executor

Spark Executor对象是负责在Spark作业中运行具体任务, 任务彼此之间相互独立; Spark应用启动时, ExecutorBackend节点被同时启动, 并且四种伴随着整个Spark应用的生命周期而存在; 有两个核心功能:

1. 负责运行组成Spark应用的任务, 并将结果返回给驱动器(Driver)
2. 它们通过自身的块管理器(Block Manager)为用户程序中要求缓存的RDD提供内存式存储, RDD是直接缓存在Executor进程内, 因此任务可以在运行时充分利用缓存数据加速运算;

### 1.2 Spark通用运行流程概述

Spark运行流程核心步骤:

1. 任务提交后, 都会先启动Driver程序;
2. 随后Driver向集群管理器注册应用程序;
3. 之后集群管理器根据此任务的配置文件分配Executor并启动;
4. Driver开始执行main函数, Spark查询为懒执行, 当执行到Action算子时开始反向推算, 根据宽依赖进行Stage的划分, 随后每一个Stage对应一个Taskset, Taskset中有多个Task, 查找可用资源Executor进行调度;
5. 根据本地化原则, Task会被分发到指定的Executor去执行, 在任务执行的过程中, Executor也会不断与Driver进行通信, 报告任务运行情况;

## 2. Spark部署模式

Spark支持多种集群管理器, 分别为:

1. Standalone: 独立模式, Spark原生的简单集群管理器, 自带完整的服务, 可单独部署到一个及群众, 无需依赖任何其他资源管理系统;
2. Hadoop YARN: 统一的资源管理机制, 在上面可以运行多套计算框架. 根据Driver在集群中的位置不同, 分为yarn client(集群外)和yarn cluster(集群内部)
3. Apache Mesos: 一个强大的分布式资源管理框架, 它允许多种不同的框架部署在其上, 包括Yarn;
4. K8S: 容器式部署环境;

### 2.1 Yarn模式运行机制

#### Yarn Cluster模式 

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420091044157.png" alt="image-20210420091044157" style="zoom: 80%;" />

1. 执行脚本提交任务, 实际是启动一个SparkSubmit的JVM进程;
2. SparkSubmit类中的main方法反射调用YarnClusterApplication的main方法;
3. YarnClusterApplication创建Yarn客户端, 然后向Yarn服务器发送执行指令: bin/java ApplicationMaster;
4. Yarn框架收到指令后会在指定的NM中启动ApplicationMaster;
5. ApplicationMaster启动Driver线程, 执行用户的作业;
6. AM向RM注册, 申请资源;
7. 获取资源后AM向NM发送指令: bin/java YarnCoarseGrainedExecutorBackend;
8. CoarseGrainedExecutorBackend进程会接收消息, 跟Driver通信, 注册已经启动的Executor, 然后启动计算对象Executor等待接收任务;
9. Driver线程继续执行完成作业的调度和任务的执行;
10. Driver分配任务并监控任务的执行;

#### Yarn Client模式

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420091203311.png" alt="image-20210420091203311" style="zoom:80%;" />

1. 执行脚本提交任务, 实际是启动一个SprakSubmit的JVM进程;
2. SparkSubmit类中的main方法反射调用用户代码的main方法;
3. 启动Driver进程, 执行用户的作业, 并创建ScheduleBackend;
4. YarnClientSchedulerBackend向RM发送指令: bin/java ExecutorLauncher;
5. Yarn框架收到指令后会在指定的NM中启动ExecutorLauncher;
6. AM向RM注册, 申请资源
7. 获取资源后AM向NM发送指令: bin/java CoarseGrainedExecutorBackend;
8. CoarseGrainedExecutorBackend进程会接收消息, 跟Driver通信, 注册已经启动的Executor, 然后启动计算对象Executor等待接收任务;
9. Driver分配任务并监控任务的执行;

### 2.2 Standalone模式

Standalone有2个重要组成部分, 分别是:

1. Master(RM): 是一个进程, 主要负责资源的调度和分配, 并进行集群的监控等职责;
2. Worker(NM): 是一个进程, 一个Worker运行在集群中的一台服务器上, 主要负责两个职责, 一个是用自己的内存存储RDD的某个或某些partition; 另一个是启动其他进程和线程(Executor), 对RDD上的partiton进行并行的处理和计算;

#### Standalone Cluster模式

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420091541185.png" alt="image-20210420091541185" style="zoom:80%;" />

#### Standalone Client模式

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420091613267.png" alt="image-20210420091613267" style="zoom:80%;" />

## 3. Spark通讯架构

### 3.1 Spark通信架构概述

**Spark通信架构发展:**

- Spark早期版本采用Akka作为内部通信部件;
- Spark1.3中引入Netty通信框架, 为了解决Shuffle的大数据传输问题使用;
- Spark1.6中Akka和Netty可以配置使用;
- Spark2系列汇总, Spark抛弃Akka, 使用Netty;

Spark通讯框架中各个组件(Client/Master/Worker)可以认为是一个个独立的实体, 各个实体之间通过消息来进行通信:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420093824629.png" alt="image-20210420093824629" style="zoom: 67%;" />

### 3.2 Spark通讯架构解析

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420094059656.png" alt="image-20210420094059656" style="zoom:67%;" />

- **RpcEndpoint**: RPC通信终端, Spark针对每个节点(Client/Master/Worker)都称之为一个RPC终端, 且都实现RpcEndpoint接口, 内部根据不同端点的需求, 设计不同的消息和不同的业务处理;
- **RpcEnv**: RPC上下文环境, 每个RPC终端运行时依赖的上下文环境成为RpcEnv;
- **Dispatcher**: 消息调度(分发)器, 针对于RPC终端需要发送远程消息或者从远程RPC接收到消息, 分发至对应的指令收件箱(发件箱);
- **Inbox**: 指令消息收件箱, 一个本地RpcEndpoint对应一个收件箱, Dispatcher在每次向Inbox存入消息时, 都将对应EndpointData加入内部ReceiverQueue中, 另外Dispatcher创建时会启动一个单独线程进行轮询ReceiverQueue, 进行收件箱消息消费;
- **RpcEndpointRef**: 是对远程RpcEndpoint的一个引用; 当我们需要向一个具体的RpcEndpoint发送消息时, 一般我们需要获取该RpcEndpoint的引用, 然后通过该引用发送消息;
- **OutBox**: 指令消息发件箱; 对于当前RpcEndpoint来说, 一个目标RpcEndpoint对应一个发件箱, 如果向多个目标RpcEndpoint发送信息, 则有多个OutBox. 当消息放入Outbox后, 紧接着通过TransportClient将消息发送过去, 消息放入发件箱以及发送过程是在同一个线程中进行的;
- **RpcAddress:** 表示远程的RpcEndpointRef的地址, Host+Port;
- **TransportClient**: Netty通信客户端, 一个OutBox对应一个TransportClient, TransportClient不断轮询OutBox, 根据OutBox消息的receiver信息, 请求对应的远程TransportServer;
- **TransportServer**: Netty通信服务端, 一个RpcEndpoint对应一个TransportServer, 接收远程消息后调用Dispatcher分发消息至对应收发件箱;

## 4. Spark任务调度机制

### 4.1 Spark任务调度概述

一个Spark应用程序包括Job,Stage以及Task三个概念:

1. Job是以Action方法为界, 遇到一个Action方法则触发一个Job;
2. Stage是Job的子集, 以RDD宽依赖(即Shuffle)为界, 遇到Shuffle做一次拆分;
3. Task是Stage的子集, 以并行度(分区数)来衡量, 分区数是多少, 则有多少个task;

**Spark的任务调度总体来说分两路进行, 一路是Stage级的调度, 一路是Task级的调度, 总体调度流程如下**

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420102711117.png" alt="image-20210420102711117" style="zoom:67%;" />

Spark RDD通过其Transactions操作, 形成RDD血缘(依赖)关系图, 即DAG, 最后通过Action的调用, 触发Job并调度执行, 执行过程中会创建两个调度器: 

- DAGScheduler负责Stage级的调度, 主要将Job切分成若干Satages, 并将每个Stage打包成TaskSet交给TaskScheduler调度
- TaskScheduler负责Task级的调度, 将DAGScheduler给过来的TaskSet按照指定的调度策略分发到Executor上执行, 调度过程中SchedulerBackend负责提供可用资源, 其中SchedulerBackend有多种实现, 分别对接不同的资源管理系统;

### 4.2 Spark Stage级调度

- Job由最终的RDD和Action方法封装而成;
- SparkContext将Job交给DAGScheduler提交, 会根据RDD的血缘关系构成的DAG进行切分, 将一个Job划分为若干Stages;
- 一个Stage是否被提交,  需要判断它的父Stage是否执行, 只有在父Stage执行完毕才能提交当前Stage; Stage提交时会将Task信息序列化并被打包成TaskSet交给TaskScheduler;

### 4.3 Spark Task级调度

TaskScheduler会将TaskSet封装为TaskSetManager加入到调度队列中, TaskSetManager负责监控管理同一个Stage中的Tasks, TaksScheduler就是以TaskSetManager为单元来调度任务;

#### 调度策略

1. **FIFO调度策略(默认)**

   按照先来先到的方式入队, 出队时直接拿出进队的TaskSetManager;

2. **FAIR调度策略**

   有一个rootPool和多个子Pool, 各个子Pool中存储着所有待分配的TaskSetManager; 先对子Pool进行排序, 再对Pool里面的TaskSetManager进行排序, 根据排序对象三个属性: runningTasks值, minShare值, weight值;

#### 本地化调度

根据每个Task的优先位置, 确定Task的Locality级别, 调度会让每个task以最高的本地性级别来启动; 当一个Task以x本地性级别启动, 但是该本地性级别对应的所有节点都没有空闲资源而启动失败, 此时并不会马上降低本地性级别, 而是在某个时间长度以内再次以x本地性级别来启动该task, 若超过限时时间则降级启动, 去尝试下一个本地性级别;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420105142621.png" alt="image-20210420105142621" style="zoom:67%;" />

#### 失败重试与黑名单机制

对于失败的Task, 会记录它失败的次数, 如果失败次数还没有超过最大重试次数, 那么就把它返回待调度的Task池子中, 否则整个Application失败;

在记录Task失败次数过程中, 会记录它上一次失败所在的Executor ID和Host, 这样下次再调度这个Task时, 会使用黑名单机制, 避免它被调度到上一次失败的节点上;

## 5. Spark Shuffle解析

### 5.1 Shuffle的核心要点

#### ShuffleMapStage与ResultStage

在划分stage时, 最后一个stage成为finalStage, 它本质上是一个ResultStage对象, 前面所有的stage被称为ShuffleMapStage;

ShuffleMapStage的结束伴随着shuffle文件的写磁盘;

ResultStage基本上对应代码中的action算子, 即将一个函数应用的RDD的各个partition的数据集上, 意味着一个job的运行结束;

### 5.2 HashShuffle解析

#### 未优化的HashShuffle

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420111310349.png" alt="image-20210420111310349" style="zoom:67%;" />

#### 优化的HashShuffle

优化的HashShuffle过程就是启用合并机制, 合并机制就是复用buffer, 开启合并机制的配置是spark.shuffle.consolidateFiles, 通常我们使用HashShuffleManager, 建议开启这个选项

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210420111515855.png" alt="image-20210420111515855" style="zoom:67%;" />

### 5.3 SortShuffle解析

#### 普通SortShuffle

在该模式下, 数据会先写入一个数据结构, reduceByKey写入Map, 一边通过Map局部聚合, 一边写入内存; Join算子写入ArrayList直接写入内存中, 然后需要判断是否达到阈值, 如果达到就会将内存数据结构的数据写入到磁盘, 清空内存数据结构;

在溢写磁盘前, 先根据key进行排序, 排序过后的数据, 会分批写入到磁盘文件中, 写入磁盘文件通过缓冲区溢写的方式, 每次溢写都会产生一个磁盘文件, 也就是说一个Task过程会产生多个临时文件;

最后每个Task中, 将所有的临时文件合并, 这就是merge过程, 此过程将所有临时文件读取出来,一次写入最终文件;

#### bypass SortShuffle

bypass运行机制的触发条件如下:

1. shuffle reduce task数量小于等于spark.shuffle.sort.bypassMergeThresholf参数的值, 默认为200;
2. 不是聚合类的shuffles算子(如reduceByKey)

task会为每个reduce端的task都创建一个临时磁盘文件, 并将数据按key进行hash然后根据key的hash值, 将key写入对应的磁盘文件中; 最后会将所有临时磁盘文件都合并成一个磁盘文件, 并创建一个单独的索引文件;

该机制与普通SortShuffleManager运行机制的不同在于: 不会进行排序; 也就是说该机制的最大好处在于, shuffle write过程中, 不需要进行数据的排序操作, 节省了这部分的性能开销;

## 6. Spark内存管理

### 6.1 堆内和堆外内存规划

Spark对JVM的堆内(On-heap)空间进行了详细的分配, 以充分利用内存, 同时Spark引入堆外(Off-heap)内存, 使之可以直接在工作节点的系统内存中开辟空间, 进一步优化内存的使用. 堆内内存受JVM统一管理, 堆外内存是直接向操作系统进行内存的申请和释放;

1. **堆内内存**
   - Executor内运行的并发任务共享JVM堆内内存, 这些任务在缓存RDD数据和广播数据时占用的内存被规划为存储(Storage)内存, 而这些任务在执行Shuffle时占用的内存被规划为执行(Execution)内存, 剩余的部分不做特殊规划;
   - Spark对堆内内存的管理是一种逻辑上的"规划式"的管理
     - 申请内存流程:
       1. Spark在代码中new一个对象实例;
       2. JVM从堆内内存分配空间, 创建对象并返回对象引用;
       3. Spark保存该对象的引用, 记录该对象占用的内存;
     - 释放内存流程如下:
       1. Spark记录该对象释放的内存, 删除该对象的引用
       2. 等待JVM的垃圾回收机制释放该对象占用的堆内内存;
2. **堆外内存**
   - 为了进一步优化内存的使用以及提高Shuffle时排序的效率, Spark引入了堆外(Off-heap)内存, 使之可以直接在工作节点的系统内存中开辟空间, 存储经过序列化的二进制数据;
   - 利用JDK Unsafe API, Spark可以直接操作系统堆外内存, 减少了不必要的内存开销, 以及频繁的GC扫描和回收, 提升了处理性能;
   - 堆外内存可以被精确地申请和释放, 是由于内存的申请和释放不再通过JVM机制, 而是直接向操作系统申请, JVM对于内存的清理是无法准确指定时间点的, 因此无法实现精确的释放;

### 6.2 内存空间分配

#### 静态内存管理

在最初的静态内存管理机制下, 存储内存,执行内存和其他内存大小在Spark应用程序运行期间均为固定的, 用户可以在启动前提前配置;

如果用户未合理配置, 容易使存储内存和执行内存中的一方剩余大量的空间, 而另一方却早早被占满; 已经很少有开发者使用;

#### 统一内存管理

Spark1.6之后引入的统一内存管理机制, 与静态内存管理的区别在于存储内存和执行内存共享同一块空间, 可以动态占用对方的空闲区域;

其中最重要的优化在于动态占用机制, 规则如下:

1. 设定基本的存储内存和执行内存区域(spark.storage.storageFraction参数), 该设定确定了双杠各自拥有的空间范围
2. 双方的空间都不足时, 则存储到硬盘, 若己方空间不足而对方空余时, 可借用对方的空间;(存储空间不足是指不注意放下一个完整的Block)
3. 执行内存的空间被对方占用后, 可让对方将占用的部分转存到硬盘, 然后归还借用的空间;
4. 存储内存的空间被对方占用后, 无法让对方"归还", 因为需要考虑Shuffle过程中的很多因素, 实现起来较为复杂;

**优点:**一定程度上提高了堆内和堆外内存资源的利用率, 降低了开发者维护Spark内存的难度;

**缺点:** 如果存储内存的空间太大或者说缓存的数据过多, 会导致频繁的全量垃圾回收, 降低任务执行时的性能, 因为缓存的RDD数据通常是长期驻留内存的;

### 6.3 存储内存管理

#### RDD的持久化机制

如果一个RDD上要执行多次行动, 可以在第一次行动中使用persist或cache方法, 在内存或磁盘中持久化或缓存这个RDD, 从而在后面的行动时提升计算速度(cache方法是使用默认的MEMORY_ONLY的存储级别将RDD持久化到内存, 故缓存是一种特殊的持久化)

RDD的持久化由Spark的Storage模块负责, 实现了RDD与物理存储的解耦合;

Spark存储级别从三个维度定义了RDD的Partition的存储方式:

- 存储位置: 磁盘/堆内内存/堆外内存
- 存储形式: Block缓存到存储内存后, 是否为非序列化的形式;
- 副本数量: 大于1时需要远程冗余备份到其他节点

#### RDD的缓存过程

RDD在缓存到存储内存之后, Partition被转换成Block, Record在堆内或堆外存储内存中占用一块连续的空间; 将Partition由不连续的存储空间转换为连续存储空间的过程, Spark称之为"展开"(Unroll);

在Unroll时要向MemoryManager申请足够的Unroll空间来临时占位, 空间不足则Unroll失败, 空间足够时可以继续进行;

如果Unroll成功, 当前Partition所占用的Unroll空间被转换为正常的缓存RDD的存储空间;

#### 淘汰与落盘

由于同一个Executor的所有的计算任务共享优先的存储内存空间, 当有新的Block需要缓存但是剩余空间不足而无法动态占用时, 就要对LinkedHashMap中的旧Block进行淘汰(Eviction), 而被淘汰的Block如果其存储级别中同时包含存储到磁盘的要求, 这要对其进行落盘(Drop),否则直接删除该Block;

**淘汰规则:**

- 被淘汰的旧Block要与新Block的MemoryMode相同, 即同属于堆外或堆内内存;
- 新旧Block不能属于同一个RDD, 避免循环淘汰;
- 旧Block所属RDD不能处于个被读状态,避免引起一致性问题
- 遍历LinkedHashMap中Block, 按照最近最少使用(LRU)的顺序淘汰, 直到满足新Block所需的空间, 其中LRU是LinkedHashMap的特性;

落盘的流程比较简单, 如果其存储界别符合\_useDisk为true的条件, 再根据其\_deserialized判断是否是非序列化的形式, 若是则对其进行序列化, 最后将数据存储到磁盘, 在Storage模块中更新其信息;

### 6.4 执行内存管理

#### Shuffle Write

- 若在map端选择普通的排序方式, 会采用ExternalSorter进行外排, 在内存中存储数据是主要占用堆内执行空间;
- 若在map端选择Tungsten的排序方式, 则采用ShuffleExternalSorter直接对以序列化形式存储的数据排序, 在内存中存储数据时可以占用堆外或堆内执行空间, 取决于用户是否开启了堆外内存以及堆外内存是否足够;

#### Shuffle Read

- 在对reduce端的数据进行聚合时, 要将数据交给Aggregator处理, 在内存中存储数据时占用堆内执行空间;
- 如果需要进行最终结果排序, 则要再次将数据交给ExternalSorter处理, 占用堆内执行空间
- 在ExternalSorter和Aggregator中, Spark会使用一种叫AppendOnlyMap的哈希表在堆内执行内存中存储数据, 但在Shuffle过程中所有数据并不能都保存到该哈希表中, 当这个哈希表占用的内存会进行周期性地采样估算, 当达到一定程度无法再从MemoryManager申请到新的执行内存时, Spark会将全部内容存储到磁盘文件中, 这个过程称为溢存(Spill), 溢存到磁盘的文件最后会被归并(Merge)

# Spark优化

## 1. Spark性能调优

### 1.1 常规性能调优

#### 最优资源配置

性能调优的第一步就是为任务分配更多的资源, 在一定范围内, 增加资源的分配与性能的提升是成正比的;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210422103515103.png" alt="image-20210422103515103" style="zoom:67%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210422103701158.png" alt="image-20210422103701158" style="zoom: 67%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210422104854404.png" alt="image-20210422104854404" style="zoom:67%;" />

#### RDD优化

1. RDD复用

   在堆RDD进行算子时, 要避免相同的算子和计算逻辑之下对RDD进行重复的计算

   <img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210422104019614.png" alt="image-20210422104019614" style="zoom:67%;" />

2. RDD持久化
   - RDD的持久化是可以进行序列化的, 当内存无法将RDD的数据完整的进行存放的时候, 可以考虑使用序列化的方式减小数据体积, 将数据完整存储在内存中;
   - 如果对于数据的可靠性要求很高,并且内存充足, 可以使用副本机制,对RDD数据进行持久化;
3. RDD极可能早的filter操作
   - 获取初始RDD后, 应该考虑尽早地过滤掉不需要的数据, 进而减少对内存的占用;

#### 并行度调节

Spark官方推荐, task数量应该设置为Spark作业总CPUcore数量的2-3倍; 这样一个task执行完毕后,CPU core会立刻执行下一个task, 降低了资源的浪费, 同时提升了Spark作业运行的效率;

并行度设置如下:

```scala
val conf = new SparkConf()
	.set("spark.default.parallelism","500")
```

#### 广播大变量

默认情况下, task中的算子中如果使用了外部的变量, 每个task都会获取一份变量的副本, 造成了内存的极大消耗

广播变量在每个Executor保存一个副本, 此Executor的所有task共用此广播变量, 这让变量产生的副本数量大大减少;

#### Kryo序列化

默认情况下,  Spark使用java的序列化机制, 效率不高, 序列化速度慢并且序列化后的数据所占用的空间依然较大;

Kryo序列化机制比Java序列化机制性能提高10倍左右, 从Spark2开始, 简单类型,简单类型数组,字符串类型的ShufflingRDDs已经默认使用Kyro序列化方式了;

#### 调节本地化等待时长

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210422110122825.png" alt="image-20210422110122825" style="zoom:67%;" />

设置本地化等待时长的代码如下:

```scala
val conf =  new SparkConf()
	.set("spark.locality.wait","6")
```

### 1.2 算子调优

#### mapPartitions

普通的map算子对RDD中的每一个元素进行操作, 而mapPartitions算子对RDD中每一个分区进行操作; 一个task只会执行一个function, function一次接收所有的partition数据, 效率比较高;

**缺点:** 数据量非常大时, function一次处理一个分区的数据, 如果一旦内存不足,. 此时无法回收内存, 就可能会OOM, 即内存溢出;

因此mapPartitions算子适用于数据量不是特别大的时候;

#### foreachPartition优化数据库操作

在生产环境中, 通常使用foreachPartition算子来完成数据库的写入, 通过其特性, 可以优化写数据库的性能;

- 对于我们写的function函数, 一次处理一整个分区的数据;
- 对于一个分区内的数据, 创建唯一的数据库连接;
- 只需要向数据库发送一次SQL语句和多组参数;

#### filter与coalesce的配合使用

使用filter对RDD中的数据进行过滤后, 每个分区的数据量有可能会存在较大差异, 存在以下问题

- 每个partition的数据量变小了, 如果还按照之前的partition相等的task个数去处理当前数据, 浪费task的计算资源;
- 每个partition的数据量不一样, 会导致后面的每个task处理每个partition数据的时候, 每个task要处理的数据量不同,  可能导致数据倾斜

可利用coalesce算子进行重分区解决以上问题;

#### repartition解决SparkSQL低并行度问题

为了解决SparkSQL无法设置并行度和task数量的问题,可以使用repartition算子;

#### reduceByKey预聚合

reduceByKey相较于普通的shuffle操作一个显著的特点就是会进行map端的本地聚合; 使用reduceByKey对性能的提升如下:

- 在map端的数据量变少, 减少了磁盘IO, 也减少了对磁盘空间的占用;
- 下一个stage拉取的数据量变少, 减少了网络传输的数据量;
- 在reduce端进行数据缓存的内存占用减少;
- 在reduce端进行聚合的数据量减少;

### 1.3 Shuffle调优

#### 调节map端缓冲区大小

map端缓冲的默认配置是32KB, 设置代码如下

```scala
val conf = new SparkConf()
	.set("spark.shuffle.file.buffer","64")
```

#### 调节reduce端拉取数据缓冲区大小

shuffle reduce task的buffer缓冲区大小决定了reduce task每次能够缓冲的数据量, 默认48MB, 设置代码如下:

```scala
val conf = new SparkConf()
	.set("spark.reducer.maxSizeInFlight","96")
```

#### 调节reduce端拉取数据重试次数

对于那些包含了特别耗时的shuffler操作的作业, 建议增加重试最大次数以避免由于JVM的full gc或网络不稳定等因素导致的数据拉取失败; 对于超大数据量(数十亿~上百亿)的shuffle过程, 调节该参数可以大幅度提升稳定性;

默认为3, 设置代码如下:

```scala
val conf = new SparkConf()
	.set("spark.shuffle.io.maxRetries","6")
```

#### 调节reduce端拉取数据等待间隔

reduce task拉取属于自己的数据时, 如果因为网络异常等原因导致失败会自动进行重试, 在一次失败后, 会等待一定的时间间隔再进行重试, 可以通过加大时间间隔时长以增加shuffle操作的稳定性;

默认值为5s, 设置代码如下

```scala
val conf = new SparkConf()
	.set("spark.shuffle.io.retryWait","60s")
```

#### 调节SortShuffle排序操作阈值

如果不需要排序操作, 可将阈值调大, 大于shuffle read task的数量, 这样在map端就不会进行排序, 减少排序的性能开销; 但会产生大量的磁盘文件;

默认值为200, 设置代码如下:

```scala
val conf = new SparkConf()
	.set("spark.shuffle.sort.bypassMergeThreshold","400")
```

### 1.4 JVM优化

#### 降低cache操作的内存占比

1. 静态内存管理机制

   Storage内存区域默认值为0.6, 即60%, 可以逐级向下递减, 设置代码如下

   ```scala
   val conf = new SparkConf()
   	.set("spark.storage.memoryFraction","0.4")
   ```

2. 统一内存管理机制

#### 调节Executor堆外内存

默认情况下, Executor堆外内存上限大概为300多MB,  可通过以下代码在spark-submit脚本里配置

```shell
--conf spark.yarn.executor.memoryOverhead=2048
```

#### 调节连接等待时长

调节连接等待时长后, 通常可以避免部分的XX文件拉取失败,XX文件lost等报错;

在spark-submit脚本中进行设置, 设置方式如下所示:

```shell
--conf spark.core.connection.ack.wait.timeout=300
```

## 2. Spark数据倾斜

主要指shuffle过程中出现的数据倾斜问题, 是由于不同的key对应的数据量不同导致的不同task所处理的数据量不同的问题;

数据倾斜与数据过量的区别: 数据倾斜是指少数task被分配了绝大多数的数据, 因此少数task运行缓慢; 数据过量是指所有task被分配的数据量都很大, 相差不多, 所有task都运行缓慢;

**数据倾斜的表现:**

- Spark作业的大部分task都执行迅速, 只有有限的几个task执行的非常慢;
- Spark作业的大部分task都执行迅速, 但是有的task在运行过程中会突然报出OOM;

**定位数据倾斜问题:**

- 查阅代码中的shuffle算子, 根据代码逻辑判断此处是否会出现数据倾斜;
- 查看Spark作业的log文件, log文件对于错误的记录会精确到代码的某一行, 可以根据异常定位到的代码位置来明确错误发生在第几个stage, 对应的shuffle算子时哪一个;

### 2.1 解决方案1:聚合元数据

1. 避免shuffle过程: 在Hive表中对数据进行聚合;
2. 缩小key粒度(增大数据倾斜可能性, 降低每个task的数据量)
3. 增大key粒度(减小数据倾斜可能性, 增大每个task的数据量)

### 2.2 解决方案2: 过滤导致倾斜的key

如果spark作业中允许丢弃某些数据, 那么可以考虑将可能导致数据倾斜的key进行过滤;

### 2.3 解决方案3: 提高shuffle操作中的reduce并行度

reduce端并行度的提高就增加了reduce端task的数量, 那么每个task分配到的数据量就会相应减少, 由此环节数据倾斜问题;

**缺陷:** 并没有从根本上改变数据倾斜的本质和问题, 只是尽可能去环节和减轻shuffle reduce task的数据压力,以及数据倾斜问题, 适用于有较多key对应的数据量都比较大的情况;

### 2.4 解决方案4: 使用随机key实现双重聚合

首先通过map算子给每个数据的key添加随机数前缀, 对key进行打上, 将原先一样的key编程不一样的key, 随后进行第一次聚合; 然后去掉每个key的前缀, 再次进行聚合;

此方法适用于由groupByKey,reduceByKey这类算子造成的数据倾斜有较好的效果, 仅适用聚合类的shuffle操作, 适用范围较窄, 如果是join类的shuffle操作, 还得用其他解决方案;

### 2.5 解决方案5: 将reduce join转换为map join

正常情况下, join操作都会执行shuffle过程, 并且执行的是reduce join, 也就是先将所有相同的key和对应的value汇聚到一个reduce task中, 然后再进行join;

如果一个RDD是比较小的, 则可以采用广播小RDD全脸数据+map算子来实现与join同样的效果, 也就是map join, 此时就不会发生shuffle操作, 也就不会发生数据倾斜;

**不适用场景分析:**

由于Spark的广播变量是在每个Executor中保存一个副本, 如果两个RDD数据量都比较大, 那么如果见一个数据量比较大的RDD做成广播变量, 那么有可能会造成内存溢出;

### 2.6 解决方案6: sample采样对倾斜key单独进行join

在Spark中, 如果某个RDD只有一个key, 那么shuffle过程中会默认将此key对应的数据打散, 由不同的reduce端task进行处理;

当由单个key导致数据倾斜时, 可将发生数据倾斜的key单独提取出来, 组成一个RDD, 然后用这个原本会导致倾斜的key组成的RDD跟其他RDD单独join

**适用场景分析:**

就一个key的数据量特别多, 可以考虑使用这种方法; 当数据量非常大时, 可以考虑使用sample采样获取10%的数据, 然后分析这10%的数据中哪个key可能会导致数据倾斜, 然后将这个key对应的数据单独提取出来;

**不适用场景分析:**

如果一个RDD总导致数据倾斜的key很多, 那么此方案不适用;

### 2.7 解决方案7:使用随机数扩容进行join

如果在进行join操作时, RDD中有大量的key导致数据倾斜, 可以考虑对其中一个RDD数据进行扩容, 另一个RDD进行稀释后再join;

**核心思想:**

- 选择一个RDD, 使用flatMap进行扩容, 对每条数据的key添加数值前缀(1~N的数值), 将一条数据映射为多条数据(扩容);
- 选择另一个RDD, 进行map映射操作, 每条数据的key都打上一个随机数作为前缀(1~N随机数);(稀释)

**局限性**

- 如果两个RDD都很大, 那么将RDD进行N倍扩容显然行不通;
- 使用扩容的方式只能缓解数据倾斜, 不能彻底解决数据倾斜问题;

**利用方案7的思想完善方案六:**

1. 对包含少数几个数据量过大的key的那个RDD, 通过sample算子采样出一份样本来, 然后统计一下每个key的数据量, 计算出来数据量最大的是哪几个key;
2. 将这几个key对应的数据从原来的RDD中拆分出来, 形成一个单独的RDD, 并给每个key都打上n以内的随机数作为前缀, 而不会导致倾斜的大部分key形成另外一个RDD;
3. 接着将需要join的另一个RDD, 也过滤出那几个倾斜key对应的数据并形成一个单独的RDD, 将每条数据膨胀成n条数据, 这n条数据都按顺序附加一个0-n的前缀, 不会导致倾斜的大部分key也形成另一个RDD;
4. 再见附加了随机前缀的独立RDD与另一个膨胀n倍的独立RDD进行join, 此时就可以将原先相同的key打散成n份, 分散到多个task中去进行join;
5. 而另外两个普通的RDD就照常join即可;
6. 最后将两次join的结果使用union算子合并起来即可, 就是最终的join结果;

## 3. Spark故障排除

详见Spark优化第**26-30**页;

