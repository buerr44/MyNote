# Hadoop入门

## 1. Hadoop概述

### 1.1 Hadoop是什么

1. Hadoop是一个由Apache基金会所开发的**分布式系统基础架构**;
2. 主要解决海量数据的**存储**和海量数据的**分析计算**问题;
3. 广义上说Hadoop通常是指更广泛的概念-Hadoop生态圈;

### 1.2 Hadoop发展历史

- Hadoop创始人**Doug Cutting**, 在Lucene框架基础上进行优化升级;
- Google是Hadoop的思想之源(大数据方面的三篇论文)

### 1.3 Hadoop三大发行版本

1. **Apache**: 最原始最基础的版本, 对于入门学习最好 - 2006;
2. **Cloudera**: 内部继承了很多大数据框架, 对应产品CDH -2008;
3. **Hortonworks**:  文档较好, 对应产品HDP, 已被Cloudera公司收购, 推出新品牌CDP - 2011;

### 1.4 Hadoop优势(4高)

1. **高可靠性**: 底层维护多个数据副本, 即使某个计算元素或存储出现故障, 也不会导致数据的丢失;
2. **高扩展性**: 在集群间分配任务数据, 可方便地扩展数以千计的节点;
3. **高效性**: 在MapReduce的思想下, Hadoop是并行工作的, 以加快任务处理速度;
4. **高容错性**: 能够自动将失败的任务重新分配;

### 1.5 Hadoop组成(重点)

#### Hadoop 1.x / 2.x / 3.x的区别

- Hadoop 1.x时代: MapReduce+HDFS+Common, 其中MR同时处理业务逻辑运算和资源的调度, 耦合性较大;
- Hadoop 2.x时代: MP+Yarn+HDFS+Common, Yarn只负责资源的调度, MR只负责运算
- Hadoop 3.x时代: 在组成上没有变化

#### HDFS架构概述

HDFS是一个分布式文件系统,

- NameNode(nn): 存储文件的元数据, 如文件名,文件目录结构,文件属性,以及每个文件的块列表和块所在的DataNode等;
- DataNode(dn): 在本地文件系统存储文件块数据,以及块数据的校验和;
- Secondary NameNode(2nn): 每隔一段时间对NameNode元数据备份;

#### Yarn架构概述

Yarn是Hadoop的资源管理其实;

- ResourceManager(RM): 整个集群资源(内存,CPU等)的老大
- NodeManager(NM): 单个节点服务资源老大;
- ApplicationMaster(AM): 单个任务运行的老大;
- Container: 容器,相当一台独立的服务器, 里面封装了任务运行所需的资源, 如内存,CPU,磁盘.网络等;

#### MapReduce架构概述

MapReduce将计算过程分为两个阶段: Map和Reduce

1. Map阶段并行处理输入数据;
2. Reduce阶段对Map结果进行汇总;

### 1.6 大数据生态体系

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210414152347590.png" alt="image-20210414152347590" style="zoom: 50%;" />

# Hadoop(HDFS)

## 1. HDFS概述

### 1.1 HDFS产出背景及定义

#### 产生背景

随着数据量越来越大, 数据分配到多个操作系统管理的磁盘中, 但不方便管理和维护, 需要一种系统来管理多态机器上的文件, 也就是分布式文件管理系统, HDFS就是分布式文件管理系统的一种;

#### 定义

HDFS: Hadoop Distributed File System, 它是一个文件系统,用于存储文件; 其次它是分布式的;

HDFS使用场景: 一次写入,多次读出的场景;

### 1.2 HDFS优缺点

#### 优点

1. **高容错性**
   - 数据自动保存多个副本, 通过增加副本的形式, 提高容错性
   - 某个副本丢失以后, 它可以自动恢复;
2. **适合处理大数据**
   - 数据规模: 能够处理数据规模达到GB,TB,甚至PB级别的数据;
   - 文件规模: 能够处理百万规模以上的文件数量;
3. **可构建在廉价机器上**, 通过多副本机制, 提高可靠性;

#### 缺点

1. **不适合低延时数据访问**, 比如毫秒级的存储数据, 是做不到的;
2. **无法高效地对大量小文件进行存储**
   - 存储大量小文件的话, 会占用NameNode大量的内存来存储文件目录和块信息;
   - 小文件存储的寻址时间会超过读取时间, 违反了HDFS的设计目标;
3. **不支持并发写入, 文件随机修改**
   - 一个文件只能有一个写, 不允许多个线程同时写;
   - 仅支持数据append(追加), 不支持文件的随机修改;

### 1.3 HDFS组成架构

1. **NameNode(nn)**: 就是Master, 它是一个主管,管理者
   - 管理HDFS的名称空间
   - 配置副本策略
   - 管理数据块(Block)映射信息
   - 处理客户端读写要求
2. **DataNode(dn)**: 就是Slavem, nn下达命令, dn执行实际的操作
   - 存储实际的数据块
   - 执行数据块的读/写操作
3. **Client**: 就是客户端
   - 文件切分: 文件上传HDFS的时候, Client将文件切分成一个个Block, 然后进行上传;
   - 与NameNode交互, 获取文件的位置信息;
   - 与DataNode交互, 读取或者写入数据
   - Client提供一些命令来管理HDFS, 比如NameNode格式化;
   - Client可以通过一些命令来访问HDFS, 如对HDFS增删改查操作;
4. **Secondary NameNode(2nn)**: 并非NameNode的热备, 当NameNode挂掉的时候, 它并不能马上替换NameNode并提供服务
   - 辅助NameNode, 分担其工作量, 比如定期合并Fsimage和Edits, 并推送给NameNode;
   - 在紧急情况下, 可辅助恢复NameNode;

### 1.4 HDFS文件块大小(重点)

HDFS文件在物理上是分块存储(Block), 块的大小可以通过配置参数dfs.blocksize来规定, 默认大小是128MB (1.x版本是64MB);

寻址时间即查找到目标Block的时间, 为传输时间的1%为最佳状态;

**为什么块的大小不能设置太小, 也不能设置太大?**

- HDFS的块设置太小, 会增加寻址时间, 程序一直在找块的开始位置;
- HDFS的块设置太大, 从磁盘传输数据的时间会明显大于定位这个块开始位置所需的时间, 导致程序在处理这块数据时会非常慢;

**总结: HDFS块的大小设置主要取决于磁盘传输速率**

## 2. HDFS的Shell操作

### 2.1 常用命令实操

#### 准备工作

- 启动Hadoop集群

```shell
[user@hadoop102 hadoop-3.1.3]$ sbin/start-dfs.sh
[user@hadoop103 hadoop-3.1.3]$ sbin/start-yarn.sh
```

- 创建/sanguo文件夹

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -mkdir /sanguo
```

#### 上传

- **-moveFromLocal**: 从本地剪切粘贴到HDFS

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -moveFromLocal ./shuguo.txt /sanguo
```

- **-copyFromLocal**: 从本地文件系统中拷贝文件到HDFS路径去

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -copyFromLocal ./weiguo.txt /sanguo
```

- **-put**: 等同于copyFromLocal, 生产环境更习惯用put

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -put ./weiguo.txt /sanguo
```

- **-appendToFile:** 追加一个文件到已经存在的文件末尾

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -appendToFile liubei.txt /sanguo/shuguo.txt
```

#### 下载

- **-copyToLocal**: 从HDFS拷贝到本地

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -copyToLocal /sanguo/shuguo.txt ./
```

- **-get**: 等同于copyToLocal, 生产环境更习惯用get

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -get /sanguo/shuguo.txt ./shuguo2.txt
```

#### HDFS直接操作

- -ls: 显示目录信息
- -cat: 显示文件呃逆荣
- -chgrp,-chmod-chown: Linuxw文件系统中的用法一样,修改文件所属权限
- -mkdir: 创建路径
- -cp: 从HDFS的一个路径拷贝到HDFS的另一个路径
- -mv: 在HDFS目录中移动文件
- -tail: 显示一个文件的末尾1kb的数据
- -rm: 删除文件或文件夹
- -rm -r: 递归删除目录及目录里面的内容
- -du: 统计文件夹的大小信息
- -setrep: 设置HDFS中文件的副本数量

```shell
[user@hadoop102 hadoop-3.1.3]$ hadoop fs -setrep 10 /jinguo/shuguo.txt
```

## 3. HDFS的API操作

详见PDF<尚硅谷大数据技术值Hadoop(HDFS)>

## 4. HDFS的读写流程(面试重点)

### 4.1 HDFS写数据流程

1. Client通过Distributed FileSystem模块向NameNode请求上传文件, NameNode检查目标文件是否已存在, 父目录是否存在;
2. NameNode返回是否可以上传;
3. 客户端请求第一个Block上传到哪几个DataNode服务器上;
4. NameNode返回三个DataNode节点, 分别为dn1,dn2,dn3;
5. 客户端通过FSDataOutputStream模块请求dn1上传数据, dn1收到请求会继续调用dn2, 然后dn2调用dn3, 将这个通信管道建立完成;
6. dn1, dn2, dn3逐级应答客户端;
7. 客户端开始往dn1上传第一个Block(先从磁盘读取数据放到一个本地内存缓存), 以Packet为单位, dn1收到一个Packet就会传给dn2, dn2传给dn3, **dn1每传一个packet会放入一个应答队列等待应答**;
8. 当一个Block传输完成之后, 客户端再次请求NameNode上传第二个Blockd的服务器(重复3-7步);

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210414163302240.png" alt="image-20210414163302240" style="zoom:67%;" />

#### 网络拓扑-节点距离计算

在HDFS写数据过程中, NameNode会选择距离待上传数据最近距离的DataNode接收数据;

**节点距离**: 两个节点到达最近的共同祖先的距离总和;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210414163607840.png" alt="image-20210414163607840" style="zoom:67%;" />

#### 机架感知(副本存储节点选择)

- 第一个副本在Client所处的节点上, 如果客户端在集群外, 随机选一个;
- 第二个副本在另一个机架的随机一个节点;
- 第三个副本在第二个副本所在机架的随机节点;

### 4.2 HDFS读取数据

1. 客户端通过DistributedFileSystem向NameNode请求下载文件, NameNode通过查询元数据, 找到文件块所在的DataNode地址;
2. 挑选一台DataNode(就近原则, 然后随机)服务器, 请求读取数据;
3. DataNode开始传输数据给客户端(从磁盘里面读取数据输入流, 以Packet为单位来做校验);
4. 客户端以Packet为单位接收, 先在本地缓存, 然后写入目标文件;

## 5. NN和2NN

### 5.1 NN和2NN工作机制

**NameNode中的元数据存储在哪?**

- 需要放在NameNode节点的内存中, 因为经常要随机访问, 需提升效率
- 但是不能只放在内存中, 一旦断电元数据就丢失了, **因此也产生在磁盘中备份元数据的FsImage(镜像文件)**;
- 内存中元数据更新时, 如果同时更新FsImage会导致效率过低, 不更新又会带来一致性问题, **因此引入Edits文件(编辑文件), 每当元数据有更新或者添加时, 修改内存中的元数据并最佳到Edits中**, 一旦NameNode节点断电, 可通过FsImage和Edits合并合成元数据;
- 但如果长时间添加数据到Edits中,会导致文件数据过大, 效率降低, 而且一旦断电恢复时间过长, 因**此引入SecondaryNameNode, 专门用于FsImage和Edits的定期合并;**

#### NameNode工作机制

1. **第一阶段: NameNode启动**
   1. 第一次启动NameNode格式化后, 创建FsImage和Edits文件. 如果不是第一次启动,直接加载编辑日志和镜像文件到内存;
   2. 客户端对元数据进行增删改的请求;
   3. NameNode记录操作日志, 更新滚动日志;
   4. NameNode在内存中对元数据进行增删改;
2. **第二阶段: Secondary NameNode工作**
   1. 2NN询问NameNode是否需要CheckPoint, 直接带回NameNode是否检查结果;
   2. Secondary NameNode请求执行CheckPoint;
   3. NameNode滚动正在写的Edits日志;
   4. 将滚动前的编辑日志和镜像文件拷贝到2NN;
   5. 2NN加载编辑日志和镜像文件到内存, 并合并;
   6. 生成新的镜像文件FsImage.chjpoint;
   7. 拷贝FsImage.chjpoint到NameNode;
   8. NameNode将FsImage.chkpoint重新命名成FsImage;

### 5.2 FsImage和Edits解析

1. FsImage文件:   HDFS文件系统元数据的一个**永久性的检查点**, 其中包含HDFS文件系统的所有目录和文件inode的序列化信息;
2. Edits文件: 存放HDFS文件系统的所有更新操作的路径, 文件系统客户端执行的所有写操作首先会被记录到Edits文件中;
3. seen_txid文件保存的是一个数字, 就是最后一个edits_的数字;
4. 每次NameNode启动的时候都会将FsImage文件读入内存, 加载Edits里面的更新操作, 保证内存中的元数据信息是最新的,同步的;

### 5.3 CheckPoint时间设置

1. 通常情况下, 2NN每个一小时执行一次;
2. 一分钟检查一次操作次数, 当操作次数达到1百万时, 2NN执行一次;

## 6. DataNode

### 6.1 DataNode工作机制

1. 一个数据块在DataNode上以文件形式存储在磁盘上, 包括两个文件, 一个是数据本身, 一个是元数据包括数据块的长度,块数据的校验和以及时间戳;
2. DataNode启动后向NameNode注册, 通过后, 周期性(6小时)的向NameNode上报所有的块信息;
3. 心跳是每3秒一次, 心跳返回结果带有NameNode给该DataNode的命令如复制块数据到另一台机器,或删除某个数据块; 如果超过10分钟没有收到某个DataNode的心跳, 则任务该节点不可用;
4. 集群运行中可以安全加入和退出一些机器;

### 6.2 数据完整性

保证DataNode数据完整性的方法步骤:

1. 当DataNode读取Block的时候, 它会计算CheckSum.
2. 如果计算后的CheckSum, 与Block创建时值不一样, 说明Block已经损坏;
3. Client读取其他DataNode上的Block;
4. 常见的校验算法crc(32), md5(128), shal(160)
5. DataNode在其文件创建后周期验证CheckSum;

### 6.3 掉线时限参数设置

1. DataNode进程死亡或者网络故障造成DataNode无法与NameNode通信
2. NameNode不会立刻把该节点判定为死亡, 要经过一段时间, 这段时间暂称作超时时长;
3. HDFS默认的超时时长为10分钟+30秒
4. 如果定义超时时间为TimeOut, 则超时时长的计算公式为

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210414175426197.png" alt="image-20210414175426197" style="zoom: 67%;" />

# Hadoop(MapReduce)

## 1. MapReduce概述

### 1.1 MapReduce定义

MapReduce是一个分布式运算程序的编程框架, 是用于开发"基于Hadoop的数据分析"的核心框架;

MapReduce的核心功能是将用户编写的业务逻辑代码和自带默认组件整合成一个完整的分布式运算程序, 并发运行在一个Hadoop集群上;

### 1.2 MapReduce优缺点

#### 优点

1. MapReduce易于编程
   - 简单实现一些借口,就可以完成一个分布式程序;
2. 良好的扩展性
   - 当计算资源不能得到满足时, 可以通过简单的增加机器来扩展计算能力
3. 高容错性
   - 其中一台机器挂了, 可以把上面的计算任务转移到另外一个节点上运行, 不至于这个任务运行失败. 且不需要人工参与;
4. 适合PB级以上海量数据的离线处理
   - 可以实现上千台服务器集群并发工作, 提供数据处理能力;

#### 缺点

1. 不擅长实时计算
   - 无法像MySQL一样, 在毫秒或者秒级内返回结果
2. 不擅长流式计算
   - 流式计算的输入数据时动态的, 而MR的输入数据集是静态的
3. 不擅长DAG(有向无环图)计算
   - 多个应用程序存在依赖关系, 后一个应用程序的输入为前一个的输出, 在这种情况下, 每个MR作业的输出结果都会写入到磁盘, 会造成大量的磁盘IO, 导致性能非常低下;

### 1.3 MapReduce核心思想

1. 分布式的运算程序往往需要分层至少2个阶段;
2. 第一个阶段的MapTask并发实例, 完全并行运行, 互不相干;
3. 第二个阶段的ReduceTask并发实例互不相干, 但是他们的数据依赖于上一个阶段的所有MapTask并发实例的输出;
4. MapReduce编程模型只能包含一个Map阶段和一个Reduce阶段, 如果用户的业务逻辑非常负责, 那就只能多个MapReduce程序,串行运行;

### 1.4 MapReduce进程

一个完整的MapReduce程序在分布式运行时有三类实例进程:

1. MrAppMaster: 负责整个程序的过程调度及状态协调;
2. MapTask: 负责Map阶段的整个数据处理流程;
3. ReduceTask: 负责Reduce阶段的整个数据处理流程;

### 1.5 MapReduce编程规范

用户编写的程序分成三个部分: Mapper, Reducer和Driver

1. Mapper阶段:

   1. 用户自定义的Mapper要继承自己的父类;
   2. Mapper的输入数据是KV对的形式(KV的类型可自定义);
   3. Mapper中的业务逻辑写在map()方法中;
   4. Mapper的输出数据是KV对的形式(KV的类型可自定义);
   5. map()方法(MapTask进程)对每一个<K,V>调用一次;

2. Reducer阶段

   1. 用户自定义的Reducer要继承自己的父类;
   2. Reducer的输入数据类型对应Mapper的输出数据类型, 也是KV;
   3. Reducer的业务逻辑写在reduce()方法中;
   4. ReduceTask进程对每一组相同k的<k,v>组调用一次reduce()方法;

3. Driver阶段

   相当于YARN集群的客户端, 用于提交我们整个程序到YARN集群, 提交的是封装了MapReduce程序相关运行参数的job对象;

## 2. Hadoop序列化

### 2.1 序列化概述

#### 什么是序列化

**序列化**是**把内存中的对象, 转换成字节序列**, 以便于存储到磁盘和网络传输;

**反序列化**就是将收到直接序列或者是**磁盘的持久化数据, 转换成内存中的对象**;

#### 为什么要序列化

"活的"对象只生存在内存里, 序列化可以存储"活的"对象, 可以将"活的"对象发送到远程计算机;

#### 为什么不用Java的序列化

Java的序列化是一个重量级序列化框架(Serializable), 会附带很多额外的信息, 不便于在网络中高效传输, 所以Hadoop自己开发了一套序列化机制(Writable)

#### Hadoop序列化特点

1. 紧凑: 高效使用存储空间
2. 快速: 读写数据的额外开销小
3. 互操作: 支持多语言交互

### 2.2 自定义bean对象实现序列化接口(Writable)

实现bean对象序列化步骤:

1. 必须实现Writable接口;

2. 反序列化时, 需要反射调用空参构造函数, 所以必须有空参构造;

   ```java
   public FlowBean(){
   	super();
   }
   ```

3. 重写序列化方法

   ```java
   @Override
   public void write(DataOutput out) throws IOException{
   	out.writeLong(upFlow);
   	out.writeLong(downFlow);
   	out.writeLong(sumFlow);
   }
   ```

4. 重写反序列化方法

   ```java
   @Override
   public void readFields(DataInput in) throws IOException{
   	upFlow = in.readLong();
   	downFlow = in.readLong();
   	sumFlow = in.readLong();
   }
   ```

5. **注意反序列化的顺序和序列化的顺序完全一致**;

6. 要想把结果像是在文件中, 需要重写toString(), 可用"\t"分开, 方便后续使用;

7. 如果需要将自定义的bean放在key中传输, 则还需要实现Comparable接口, 因为MapReduce框中的Shuffle过程要求对key必须能排序

   ```java
   @Override
   public int compareTo(FlowBean o){
   	//倒序排列,从大到小
   	return this.sumFlow > o.getSumFlow() ? -1 : 1;
   }
   ```

## 3. MapReduce框架原理

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415094332782.png" alt="image-20210415094332782" style="zoom:67%;" />

### 3.1 InputFormat数据输入

#### 切片与MapTask并行度决定机制

MapTask的并行度决定Map阶段的任务处理并发度, 进而影响整个Job的处理速度;

**数据块**: Block是HDFS物理上把数据分层一块一块, **数据块是HDFS存储数据单位**;

**数据切片**: 数据切片只是在逻辑上对输入进行分片, 并不会在磁盘上将其切分成片进行存储; **数据切片是MapReduce程序计算输入数据的单位,** 一个切片会对应启动一个MapTask;

默认情况下, 切片大小 = BlockSize;

#### FileInputFormat切片机制

1. 简单地按照文件的内容长度进行切片
2. 切片大小, 默认等于Block大小
3. 切片室不考虑数据集整体, 而是逐个针对每一个文件单独切片;

#### TextInputFormat

- FileInputFormat实现类:

  FileInputFormat常见的接口实现类包括: TextInputFormat, KeyValueTextInputFormat, NLineInputFormat, CombineTextInputFormat和自定义InputFormat等;

- TextInputFormat

  是默认的FileInputFormat实现类, 按行读取每条记录. **键是存储该行在整个文件中的其实直接偏移量, LongWritable类型. 值时这行的内容, 不包括任何行终止符(换行符和回车符), Text类型**;

#### CombineTextInputFormat切片机制

框架默认的TextInputFormat切片机制是对任务按文件规划切片, 不管文件多小, 都会是一个单独的切片, 都会交给一个MapTask, 如果有大量小文件, 就会产生大量的MapTask, 处理效率极其低下;

CombineTextInputFormat用于小文件过多的场景, 它可以将多个小文件从逻辑上规划到一个切片中, 这样多个小文件就可以交给一个MapTask处理;

生成切片过程包括: 虚拟存储过程和切片过程二部分:

1. 虚拟存储过程:

   将输入木落下所有文件大小, 依次和设置的setMaxInputSplitSize值比较, 如果不大于设置的最大值, 逻辑上划分一个快; 如果输入文件大于设置的最大值且大于两倍, 那么以最大值切割一块; 当**剩余数据大小超过设置的最大值且不大于最大值2倍, 此时将文件均分成2个虚拟存储块(防止出现太小切片)**;

2. 切片过程:
   - 判断虚拟存储的文件大小是否大于setMaxInputSplitSize值, 大于等于则单独形成一个切片;
   - 如果不大于则跟下一个虚拟存储文件进行合并, 共同形成一个切片;

### 3.2 MapReduce工作流程

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415101457058.png" alt="image-20210415101457058" style="zoom: 67%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415101609454.png" alt="image-20210415101609454" style="zoom:67%;" />

上面的流程是整个MapReduce最全工作流程, 但是Shuffle过程知识从第7步开始到第16步结束, 具体Shuffle过程如下:

1. MapTask收集我们的map()方法输出的kv对, 放到内存缓冲区中;
2. 从内存缓冲区不断溢出本地磁盘文件, 可能会溢出多个文件;
3. 多个溢出文件会被合并成大的溢出文件;
4. 在溢出过程及合并的过程中, 都要调用Partitioner进行分区和针对key进行排序;
5. ReduceTask根据自己的分区号, 去各个MapTask机器上去相应的结果分区数据;
6. ReduceTask会抓取到同一个分区的来自不同MapTask的结果文件, ReduceTask会将这些文件再进行合并(归并排序)
7. 合并成大文件后, Shuffle过程也就结束了, 后面进入ReduceTask的逻辑运算过程

**注意:**

- Shuffle中的缓冲区大小会影响到MapReduce程序的执行效率, 原则上说, 缓冲区越大, 磁盘io的次数越少, 执行速度越快;
- 缓冲区大小可通过参数调整, 参数mapreduce.task.io.sort.mb默认100M;

### 3.3 Shuffle机制

Map方法之后, Reduce方法之前的数据处理过程称之为Shuffle;

#### Partition分区

默认分区是根据key的hashCode对ReduceTasks个数取模得到的, 用户没法控制哪个key存储到哪个分区;

**自定义Partition步骤**

1. 自定义类型继承Partitioner, 重写getPartition()方法;

2. 在Job驱动中, 设置自定义Partitioner: 

   job.setPartitionerClass(CustomPartitioner.class);

3. 自定义Partition后, 要根据自定义Partitioner的逻辑设置相应数量的ReduceTask;

   job.setNumReduceTasks(5);

**分区总结:**

1. 如果ReduceTask数量 > getPartition的结果数, 则会多产生几个空的输出文件part-r-000xx;
2. 如果1 < ReduceTask的数量 < getPartition的结果数, 则有一部分分区数据无处安放, 会Exception;
3. 如果ReduceTask的数量=1, 则不管MapTask端输出多少个分区文件, 最终结果都会交给这个ReduceTask, 最终也就只会产生一个结果文件part-r-00000;
4. 分区号必须从0开始, 逐一累加;

#### WritableComparable排序

**排序概述**

排序是MapReduce框架中最重要的操作之一;

MapTask和ReduceTask均会对数据**按照key**进行排序, 该操作属于Hadoop默认操作, **任何应用程序中的数据均会被排序, 而不管逻辑上是否需要**.

默认排序是按照**字典顺序排序**, 且实现该排序的方法是**快速排序**;

- 对于MapTask, 它会将处理的结果暂时放到环形缓冲区中, 当环形缓冲区使用率达到一定阈值后, 再对缓冲区中的数据进行一次快速排序, 并将这些有序数据溢写到磁盘上, **当数据处理完毕后. 会对磁盘上所有文件进行归并排序**;
- 对于ReduceTask, 它从每个MapTask上远程拷贝相应的数据文件, 如果文件大小超过一定阈值, 则溢写磁盘上, 否则存储在内存中; 磁盘文件数目达到一定阈值, 则进行一次归并排序生成一个更大的文件; 内存中文件大小或者数目超过一定阈值, 则进行一次合并后将数据溢写到磁盘上; **当数据拷贝完成后, ReduceTask统一对内存和磁盘上的所有数据进行一次归并排序;**

**排序分类**:

1. **部分排序**: MR根据输入记录的键对数据集排序, 保证输出的每个文件内部有序;
2. **全排序**: 最终输出结果只有一个文件, 且文件内部有序, 实现方式只设置一个ReduceTask, 处理大型文件时效率极低;
3. **辅助排序**: 在接收的key为bean对象时, 想让一个或几个字段相同的key进入到同一个reduce方法时, 可以采用分组排序;
4. **二次排序**: 在自定义排序过程中, 如果compareTo中的判断条件为两个, 即二次排序;

**自定义WritableComparable排序**

bean对象为key传输, 需要实现WritableComparabl接口重写compareTo方法, 就可以实现排序

```java
@Override
public int conpareTo(FlowBean bean){
	int result;
	
	//按照总流量大小, 倒序排序
	if(this.sumFlow > bean.getSumFlow()){
		result = -1;
	}else if(this.sumFlow < bean.getSumFlow()){
		result = 1;
	}else{
		result = 0;
	}
	
	return result;
}
```

#### Combiner合并

1. Combiner是MR程序中Mapper和Reducer之外的一种主键;
2. Combiner组件的父类就是Reducer;
3. Combiner和Reducer的区别在于运行的位置
   - **Combiner是在每一个MapTask所在的节点运行;**
   - **Reducer是接收全局所有Mapper的输出结果;**
4. Combiner的意义就是对每一个MapTask的输出进行局部汇总, 以减小网络传输量;
5. **Combiner能够应用的前提是不能影响最终的业务逻辑**, 而且Combiner的输出kv应该跟Reducer的输入kv类型对应起来;

### 3.4 OutputFormat数据输出

#### OutputFormat接口实现类

OutputFormat是MapReduce输出的基类, 所有实现MapReduce输出都实现了OutputFormat接口

默认输出格式TextOutputFormat

**自定义OutputFormat:**

应用场景: 输出数据到MySQL/HBase/Elasticsearch等存储框架中

自定义OutputFormat步骤:

1. 自定义一个类继承FileOutputFormat;
2. 改写RecordWriter, 具体改写输出数据的方法write();

### 3.5 MapReduce内核源码解析

#### MapTask工作机制

Read阶段 + Map阶段 + Collect阶段 + 溢写阶段 + Merge阶段

1. **Read阶段**: MapTask通过InputFormat获得的RecordReader, 从输入InputSplit中解析出一个个key/value;
2. **Map阶段**: 该节点主要是将解析出的key/value交给用户编写map()函数处理, 并产生一系列新的key/value;
3. **Collect收集阶段**: 在用户编写map()函数中, 当数据处理完成后, 一般会调用OutputCollector.collect()输出结果; 在该函数内部, 会将生成的key/value分区(调用Partitioner), 并写入一个环形内存缓冲区中;
4. **Spill阶段**: 即"溢写", 当环形缓冲区满后, MapReduce会将数据写到本地磁盘上, 生成一个临时文件; 需要注意的是, 将数据写入本地磁盘之前, 先要对数据进行一次本地排序, 并在必要时对数据进行合并,压缩等操作; 溢写阶段详情:
   - 步骤1: 利用快速排序算法对缓存区内的数据进行排序(先按照分区编号Partition, 再按照key进行排序)
   - 步骤2: 按照分区编号由小到大依次将每个分区中的数据写入任务工作目录下的临时文件output/spillN.out(N标识当前溢写次数)中;
   - 步骤3: 将分区数据的元信息写到内存索引数据结构SpillRecord中, 其中每个分区的元信息包括在临时文件中的偏移量,压缩前数据大小和压缩后数据大小;
5. **Merge阶段**: 当所有数据处理完成后, MapTask对所有临时文件进行一次合并, 以确保最终只会生成一个数据文件

当所有数据处理完成后, MapTask会将所有临时文件合并成一个大文件,并保存到文output/file.out中, 同时生成相应的索引文件output/file.out.index;

在向您文件合并过程中, MapTask以分区为单位进行合并, 对于某个分区, 它将采用多轮递归合并的方式;

#### ReduceTask工作机制

Copy阶段 + Sort阶段 + Reduce阶段

1. **Copy阶段**: ReduceTask从各个MapTask上远程拷贝一片数据, 并针对某一片数据, 如果其大小超过一定阈值, 则写到磁盘上, 否则直接放到内存中;
2. **Sort阶段**: 在远程拷贝数据的同时, ReduceTask启动了两个后台线程对内存和磁盘上的文件进行合并, 以防止内存使用过多或磁盘上文件过多; 由于各个MapTask已经实现对自己的处理结果进行了局部排序, 因此ReduceTask只需对所有数据进行一次归并排序即可;
3. **Reduce阶段**: reduce()函数将结算结果写到HDFS上;

#### ReduceTask并行度决定机制

MapTask并行度由切片个数决定, 切片个数由输入文件和切片规则决定, 那么ReduceTask并行度由谁决定呢?

1. 设置ReduceTask并行度(个数)

   ReduceTask的并行度同样影响整个Job的执行并发度和执行效率, 但与MapTask的并发数由切片数决定不同, ReduceTask数量的决定是可以直接手动设置:

   ```java
   //默认是1, 手动设置是4
   job.setNumReduceTasks(4);
   ```

2. ReduceTask个数多少合适
   - ReduceTask = 0, 表示没有Reduce阶段, 输出文件个数和Map个数一致;
   - ReduceTask默认值就是1, 所以输出文件个数为1;
   - 如果数据分布不均匀, 就有可能在Reduce阶段产生数据倾斜;
   - ReduceTask数量并不是任意设置, 还要考虑业务逻辑需求, 有些情况下, 需要计算全局汇总结果, 就只能有1个ReduceTask;
   - 如果分区数不是1, 但是ReduceTask为1, 是否执行分区过程? 答案是: 不执行分区过程, 因为在MapTask源码中, 执行分区的前提是先判断ReduceNum个数是否大于1, 不大于1肯定不执行;

### 3.6 Join应用

#### Reduce Join

Map端主要工作: 为来自不同表或文件的key/value对, **打标签以区别不同来源的记录, 然后用连接字段作为key**, 其余部分和新家的标志作为value, 最终进行输出;

Reduce端主要工作: 在Reduce端以连接字段作为key的分组已经完成, 只需要在每一个分组中将那些来源于不同文件的记录分开, 最后进行合并就ok了;

#### Map Join

**使用场景**

Map Join适用于一张表十分小, 一张表很大的场景

**优点**

在Map端缓存多张表, 提前处理业务逻辑, 这样增加Map端业务, 减少Reduce端数据的压力, 尽可能的减少数据倾斜;

**具体办法**

采用DistributedCache

1. 在Mapper的setup阶段, 将文件读取到缓存集合中;
2. 在Driver驱动类中加载缓存;

### 3.7 数据清洗(ETL)

ETL是英文Extract-Transform-Load的缩写, 用来描述将数据从来源端经过抽取(Extract), 转换(Transform), 加载(Load)至目的端的过程;

在运行核心业务MapReduce程序之前, 往往要相对数据进行清洗, 清理掉不符合用户的数据. **清理的过程往往只需要运行Mapper程序, 不需要运行Reduce程序;**

### 3.8 MR开发总结

1. **输入数据接口: InputFormat**
   - 默认使用的实现类: TextInputFormat;
   - TextInputFormat的功能逻辑是: 一次读一行文本, 然后将改行的起始偏移量作为key, 行内容作为value返回;
   - CombineTextInputFormat可以把多个小文件合并成一个切片处理, 提高处理效率;
2. **逻辑处理接口: Mapper**
   - 用户根据业务需求实现其中三个方法: map(), setup(), cleanup();
3. **Partitioner分区**
   - 有默认实现HashPartitioner, 逻辑是根据key的哈希值和numReduces来返回一个分区号; 
   - 如果业务上有特别的需求, 可以自定义分区
4. **Comparable排序**
   - 当我们用自定义的对象作为key来输出时, 就必须要实现WritableComparable接口, 重写其中的compareTo()方法;
   - 部分排序: 对最终输出的每一个文件进行内部排序;
   - 全排序: 对所有数据进行排序, 通常只有一个Reduce
   - 二次排序: 排序的条件有两个;
5. **Combiner合并**
   - Combiner合并可以提高程序执行效率, 减少IO传输, 但是使用时必须不能影响原有的业务处理结果;
6. **逻辑处理接口: Reducer**
   - 用户根据业务需求实现其中三个方法: reduce(), setup(), cleanup();
7. **输出数据接口: OutputFormat**
   - 默认实现类时TextOutputFormat, 功能逻辑是: 将每一个KV对, 向目标文本文件输出一行;
   - 用户还可以自定义OutputFormat;

## 4. Hadoop数据压缩

### 4.1 概述

#### 压缩的好处和坏处

**优点:** 以减少磁盘IO, 减少磁盘存储空间;

**缺点**: 增加CPU开销;

#### 压缩原则

1. 运算密集型的Job, 少用压缩;
2. IO密集型的Job, 多用压缩;

### 4.2 MR支持的压缩编码

#### 压缩算法对比介绍

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415143256645.png" alt="image-20210415143256645" style="zoom:67%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415143313809.png" alt="image-20210415143313809" style="zoom:67%;" />

#### 压缩性能比较

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415143433928.png" alt="image-20210415143433928" style="zoom:67%;" />

### 4.3 压缩方式选择

压缩方式选择时重点考虑: **压缩/解压缩速度, 压缩率(压缩后存储的大小), 压缩后是否可以支持切片**

- **Gzip压缩**
  - 优点: 压缩率比较高;
  - 缺点: 不支持Split, 压缩/解压速度一般;
- **Bzip2压缩**
  - 优点: 压缩率高, 支持Split;
  - 缺点: 压缩/解压速度慢
- **Lzo压缩**
  - 优点: 压缩/解压速度比较快, 支持Split;
  - 缺点: 压缩率一般; 想支持切片需要额外创建索引;
- **Snappy压缩**
  - 优点: 压缩和解压缩速度快
  - 缺点: 不支持Split, 压缩率一般;

#### 压缩位置选择

压缩可以在MR作用的任意阶段启用

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415144214070.png" alt="image-20210415144214070" style="zoom:67%;" />

# Hadoop(Yarn)

## 1. Yarn资源调度器

Yarn是一个资源调度平台, 负责为运算程序提供服务器运算资源, 相当于一个分布式的操作系统平台, 而MR等运算程序则相当于运行于操作系统上的应用程序;

### 1.1 Yarn基础架构

Yarn主要由ResourceManager, NodeManager, ApplicationMaster和Container等组件构成;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415144815762.png" alt="image-20210415144815762" style="zoom:67%;" />

### 1.2 Yarn工作机制

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210415150427724.png" alt="image-20210415150427724" style="zoom:67%;" />

### 1.3 作业提交全过程

- **作业提交**

  1. Client调用job.waitForCompletion方法, 向整个集群提交MapReduce作业;
  2. Client向RM申请一个id;
  3. RM给Client返回该job资源的提交路径和作业id
  4. Client提交jar包, 切片信息和配置文件到指定的资源提交路径;
  5. Client提交完资源后, 向RM申请运行MrAppMaster;

- **作业初始化**

  6. 当RM收到Client请求后, 将该job添加到容器调度器中;
  7. 某一个空闲的NM领取到该Job;
  8. 该NM创建Container, 并产生MrAppMaster;
  9. 下载Client提交的资源到本地;

- **任务分配**

  10. MrAppMaster向RM申请运行多个MapTask任务资源
  11. RM将运行MapTask任务分配给另外两个NodeManager, 另外两个NodeManager分别领取任务并创建容器;

- **任务运行**

  12. MR向两个接收到任务的NodeManager发送程序启动脚本, 这两个NM分别启动MapTask, MapTask对数据分区排序;
  13. MrAppMaster等待所有MapTask运行完毕后, 向RM申请容器, 运行ReduceTask;
  14. ReduceTask向MapTask获取相应分区的数据;
  15. 程序运行完毕后, MR会向RM申请注销自己;

- **进度和状态更新**

  Yarn中的任务将其进度和状态返回给应用管理器, 客户端每秒向应用管理器请求进度更新, 展示给用户;

- **作业完成**

  除了向应用管理器请求作业进度外, 客户端每5秒都会通过调用waitForCompletion()来检查作业是否完成. 作业完成之后, 应用管理器和Container会清理工作状态, 作业的信息会被作业历史服务器存储以备之后用户核查

### 1.4 Yarn调度器和调度算法

目前Hadoop作业调度器主要有三种: FIFO, 容量(Capacity Scheduler)和公平(Fair Scheduler);

Apache Hadoop 3.1.3 默认的资源调度器是Capacity Scheduler;

CDH框架默认调度器是Fair Scheduler;

#### 先进先出调度器FIFO

FIFO调度器(First In First Out): 单队列, 根据提交作业的先后顺序, 先来先服务;

**优点**: 简单易懂

**缺点**: 不支持多队列, 生产环境很少使用;

#### 容量调度器(Capacity Scheduler)

Capacity Scheduler是Yahoo开发的多用户调度器, 特点如下:

1. **多队列**: 每个队列可配置一定的资源量, 每个队列采用FIFO调度策略;
2. **容量保证**: 管理员可为每个队列设置资源最低保证和资源使用上限;
3. **灵活性**: 如果一个队列中的资源有剩余, 可以暂时贡献给哪些需要资源的队列, 而一旦该队列有新的应用程序提交, 则其他队列借调的资源会归还给该队列;
4. **多租户**: 支持多用户共享集群和多应用程序同时运行; 为了防止统一用户的作业独占队列中的资源, 该调度器会对同一用户提交的作业所占资源进行限定;

**资源分配算法**:

1. 队列资源分配: 从root开始, 使用深度优先算法, **优先选择资源占用率最低**的队列分配资源;
2. 作业资源分配: 默认按照提交作业的**优先级**和**提交时间**顺序分配资源;
3. 容器资源分配: 按照容器的优先级分配资源, 如果优先级相同按照数据本地性原则:
   - 任务和数据在同一节点
   - 任务和数据在同一机架
   - 任务和数据不在同一节点也不再同一机架

#### 公平调度器(Fair Scheduler)

Fair Scheduler是Facebook开发的多用户调度器, 特点如下

**与容量调度器相同点**

1. 多队列,
2. 容量保障
3. 灵活性
4. 多租户

**与容量调度器不同点**

1. 核心调度策略不同
   - 容量调度器: 优先选择**资源利用率低**的队列
   - 公平调度器: 优先选择**对资源的缺额比较大**的;
2. 每个队列可以单独设置资源分配方式
   - 容量调度器: FIFO, DRF
   - 公平调度器: FIFO, FAIR, DRF

**缺额**:

- 公平调度器设计的目标是: 在时间尺度上, 所有作业获得公平的资源; 某一时刻一个作业应获资源和实际获取资源的差距叫"**缺额**"
- 调度器会邮箱为缺额大的作业分配资源;

**公平调度器队列资源分配方式**

1. **FIFO策略**

   公平调度器每个队列资源分配策略如果选择FIFO, 此时公平调度器相当于容量调度器;

2. **Fair策略**

   是一种基于最大最小公平算法实现的资源多路复用方式(默认使用此策略), 具体资源分配流程和容量调度器一致

   1. 选择队列
   2. 选择作业
   3. 选择容器

