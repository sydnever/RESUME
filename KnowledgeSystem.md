# 知识体系

## 测试

### 测例的格式

| 项目         | 内容 |
|------------|------|
| 编号         |      |
| 测试目的     |      |
| 前置条件     |      |
| 输入         |      |
| 期望输出     |      |
| 后置条件     |      |
| 执行过程记录 |      |
| 测试日期     |      |
| 软件版本     |      |
| 测试执行者   |      |
| 测试结果     |      |

### 测试的范围

* 规格说明书所描述的期望行为
* 程序实际执行的行为
* 测试用例覆盖到的行为

### 测试类型

#### 功能测试

黑盒测试：只根据规格说明书编写的测试。确认规定行为都已实现。建立可信度。

方法：

| 方法         | 说明                                                                 |
|------------|--------------------------------------------------------------------|
| 边界值分析   | 利用输入变量的最小值，略大于最小值的值，正常值，略小于最大值的值，最大值 |
| 健壮性测试   | 超出输入范围，输入略大于最大值的值，略小于最小值的值                   |
| 最坏情况分析 | 将所有输入变量的边界值组合在一起，做笛卡尔积                          |
| 特殊值测试   | 来自经验与知识获得的特殊值                                           |
| 随机测试     | 根据输入边界设定随机数范围                                           |
| 等价类测试   | 进行某种意义上完备的测试，并尽可能减少冗余测试                        |
| 基于决策表   | 最为严格的的测试，根据输入与输出画决策矩阵                            |

#### 结构测试

白盒测试：基于程序的设计编写的测试。规避未定义行为。找出故障。

| 方法       | 说明             |
|----------|------------------|
| 路径测试   | 画代码决策路径图 |
| 数据流测试 |                  |

#### 性能测试

系统监控

性能分析：perf 火焰图

测试工具：loadrunner jmeter benchmarksql sysbench

测试模型

#### 交互性测试



### 测试的层次

| 层级         | 测试     |
|------------|----------|
| 需求规格说明 | 系统测试 |
| 概要设计     | 集成测试 |
| 详细设计     | 单元测试 |
| 编码         |          |

## 数据库

### 数据库的意义

直接用文件系统储存组织信息的弊端：

* 冗余：相同的信息可能存储在多个文件中
* 不一致：冗余存储的信息很可能得不到同步更新，而导致最终数据不一致
* 访问困难：传统文件处理环境不支持一种方便高效的方式获取所需数据
* 数据孤立：数据分散在不同文件中，难以进行快速检索
* 完整性：难以保证一些一致性约束
* 原子性：难以保证数据的更新是原子操作
* 并发访问：多用户访问同一数据进行修改时，容易产生错误结果
* 安全性：所有能访问文件所在计算机的用户都能查看文件内容‘’

### 数据抽象

1. 物理层：数据实际是如何存储的
2. 逻辑层：数据与数据之间的关系模型（数据库中的数据关系，DBA需要掌握）
3. 视图层：从逻辑层中提取部分再进一步抽象（用户访问数据库时，只需要关心自己所需的那一部分数据）

### 实例与模式

1. 实例：数据库系统中信息的集合
2. 模式：数据库的总体设计
    1. 物理模式
    2. 逻辑模式

应用程序不需要依赖物理模式，只需要依赖逻辑模式。

### 数据模型

描述数据、数据联系、数据语义以及一致性约束的概念集合。提供了一种描述物理层、逻辑层、视图层数据库设计的方式。

1. 关系模型：
2. 实体-联系模型
3. 基于对象的模型
4. 半结构化模型

### 数据库语言

1. DML 数据操纵语言
2. DDL 数据定义语言
3. DQL

### 关系数据库

表

### 数据库设计

### 存储和查询

#### 存储

组件：

1. 权限和完整性
2. 事务
3. 文件管理
4. 缓冲区管理

数据结构：

1. 数据文件
2. 数据字典：元数据
3. 索引：便于快速访问

#### 存储实现

b-tree，写放大

LSM：随机写转变为顺序写，有个内存表能够快速判断这一行是不是在这个sst文件中

#### 数据分布

衡量数据分布算法的标准：

* 均匀性：不同存储节点的负载应该均衡
* 稳定性：即使在存储节点发生变化的情况下，数据根据算法计算出的分布也应该基本稳定

两者在一定程度上是相互矛盾的：在集群拓扑变化时，过分追求稳定性，注定会导致数据的不均匀；过分追求均匀，那么就必须进行大范围大数据量的数据重分布。

其他在工程实现中必须考量的其他问题：

* 性能可扩展性：数据分布算法不应该在集群规模扩大后显著增加运行时间。
* 节点异构问题：实际场景中，不同存储节点的性能与容量可能存在较大差异，最好能够提供加权的数据均匀
* 隔离故障域：尽可能减小集群发生故障时对算法结果产生的影响

**hash**：

直接用key对存储节点数量取模，均匀性取决于key值的分布，当有节点加入或退出时，稳定性毫无保证。

**一致性hash**：

第一步，对2的32方取模，整个Hash空间组织成一个虚拟的圆环，Hash函数的值空间为0 ~ 2^32 - 1(一个32位无符号整型)。

第二步，我们将各个服务器使用Hash进行一个哈希，具体可以选择服务器的IP或主机名作为关键字进行哈希，这样每台服务器就确定在了哈希环的一个位置上。

第三步，将数据Key使用相同的函数Hash计算出哈希值，并确定此数据在环上的位置，从此位置沿环顺时针查找，遇到的服务器就是其应该定位到的服务器。

在集群节点太少时，很可能由于对哈希环的分割不均匀导致数据倾斜——解决办法是对每个节点都计算出多个在哈希环上的位置。

在集群节点数量发生变化时，会造成一定的数据不均匀。

**带负载上限的一致性hash**：

hash环上每个节点都有一个可以动态调整的负载上限，当数据被分到达到负载上限的节点时，会按照沿hash环寻找下一个节点。使用者可以通过调整负载上限来达到均匀性与稳定性间的权衡。解决不了存储节点异构问题。

**带虚拟节点的一致性hash**：

节点的存储权重通过该节点在hash环上的节点数量来决定。当存储节点加入退出时，由于存储权重可能发生变化，hash环上的节点分布可能需要重新计算

**分片的一致性hash**：

将hash环进行若干等分，将每个小扇区分配给不同的节点。在存储节点加入退出的时候，将无主的扇区根据权重分配给其他节点。

**CRUSH**：

#### 查询执行

优化器：

1. 接收并验证SQL语句的语法语义
2. 分析环境并优化满足SQL语句的方法
3. 创建计算机可读指令来执行优化的SQL
4. 执行指令或存储指令直到执行

语句执行流程：

1. 语法分析：是否有语法错误
2. 语义检查：语义是否正确，如是否有相应的表相应的列
3. 查询重写：将一条语句改写成更高效的语义等价语句
4. 优化访问计划
5. 生成可执行代码
6. 执行访问计划

解释器

执行引擎

#### 优化器

我们期望优化器能找到一个最小代价的正确执行方案，但这是一个NP完全问题，所以没有任何一个优化器能在任何情况下都找到真正的最佳方案。

基础概念：

* 优化器粒度
    * 单查询
        * 更小的搜索空间
        * 查询结果无法重用
        * 代价模型需要考虑正在运行的内容，来避免资源争夺问题
    * 多查询
        * 查询结果可重用，在类似查询较多情况下，效率会很高
        * 搜索空间非常大
        * 多个查询会共用一个扫描表的结果
* 优化策略：
    * 静态优化
        * 在执行前选择最佳计划
        * 计划质量取决于代价模型的准确性
    * 动态优化
        * 主要是处理流式数据时会使用
        * 查询执行时，即时选择执行计划
        * 难以实现与调试
    * 混合优化
        * 静态算法编译
        * 如果实际运行计划偏差太大，会重新优化
* 固定执行计划的方法
    * Hint
        * 通过一些特殊的SQL语法指定具体的执行计划
    * 通过优化选项开关或是单独回滚优化器的版本

执行计划搜索策略：
* 基于规则的：
    * 将一些外连接转成内连接
    * 子查询exist转成in，in再转成semijoin
    * 否定消除
    * 等值常量传递，以便尽早下推
    * 常量表达式计算出结果
* 启发式
    * 通过一些规则来做静态的优化
        * 尽早进行过滤，越严格的限制应越早执行
        * 在Join前执行所有过滤
        * 谓词/限制/投影下推
        * 等值条件处理
    * 优点
        * 易于实现和调试
        * 对于简单查询很有效
    * 缺点
        * 会依靠玄学常数预测计划的有效性
        * 当操作间存在复杂的相互依赖关系时，几乎没法得出好的执行计划
* 基于代价优化：
    * 判断选择哪个索引
    * 确定Join顺序
    * 确定子查询执行策略
* 启发式+基于代价的Join顺序
    * 静态规则完成初始优化
    * 动态规划确定多表join的最佳顺序
        * 分治法自底向上搜索

代价估算模型：

* 代价类型
    * 物理代价
        * CPU资源，I/O，缓存命中率，内存等
    * 逻辑代价
        * 估计每个操作算子产生的结果大小
    * 算法代价
        * 算法实现复杂度
* 基于磁盘的数据库代价模型
    * 磁盘访问情况将决定整条语句的查询时间
        * CPU代价在整个代价模型中占比较小
        * 容易计算出每个I/O的代价
        * 分布式数据库场景下，网络I/O的代价占比也比较高
* Postgres 代价模型
    * CPU与I/O的代价通过玄学常数加权估计
    * 默认计算模型是没有大量内存的磁盘数据库
    * 从内存中获取数据比磁盘中快400倍
    * 顺序I/O比随机I/O快4倍
* IBM DB2 代价模型
    * 结合了多种代价模型
        * 硬件环境的基准值：存储、网络、内存
        * 并发环境：连接数、隔离级、可用锁数量
* 基于内存的数据库代价模型

直方图统计信息：

* 类型
    * 等高
    * 等宽
* 直方图中每个桶包含内容
    * 数值上下界
    * 在总数中的占比
    * 不同值的个数
* 用途
    * 帮助估计filter算子得到的数据总量，为后续优化提供基础
* 维护
    * 实时维护代价比较高
    * 在线统计动作可能会让数据库产生性能波动

并行Join算法：





### 事务管理

事务：完成单一逻辑功能的操作集合。

A：原子不可分
C：保证一致性约束
I：隔离并发互相不影响
D：持久不丢失

恢复管理器：保证A、D
并发控制管理器：保证I

#### 隔离级别

https://zhuanlan.zhihu.com/p/43621009


#### 事务隔离与锁

基于行锁，与两阶段锁。

两阶段锁：一个事务之内分为两个阶段，加锁阶段和解锁阶段，加锁阶段在前只能加锁，解锁阶段在后只能解锁，在事务结束后完成解锁。

排他锁：禁止读写行为。

共享锁：禁止写不禁止读。

| 等级                 | 描述                                                                    | 解决问题             |
|--------------------|-----------------------------------------------------------------------|--------------------|
| 一级锁（RU）           | 写操作前上排他锁，事务结束才可释放排他锁                                 | 脏写                 |
| 二级锁（RC）           | 写操作前上排他锁，读操作上共享锁，读完可释放共享锁，事务结束才可释放排他锁 | 脏写，脏读            |
| 三级锁（Serializable） | 写操作前上排他锁，读操作上共享锁，事务结束才可释放排他锁和共享锁          | 脏读，脏写，不可重复读 |

RR隔离级的定义比较模糊，各个数据库并没有达成统一实现。

幻读需要间隙锁/谓词锁来解决。

#### 事务隔离与MVCC

1. 每个事务都有全局唯一的trx_id
2. 写入数据库的每条记录，都应有隐藏列trx_id作为版本号，记录的可读性由版本号与当前事务的trx_id之间的关系决定。

trx_id有序的情况下：

| 解决问题   | 具体操作                                                           |
|----------|----------------------------------------------------------------|
| 脏写       | 已有记录的版本号大于当前trx_id时无法写入新的记录                   |
| 脏读       | 只能读到版本号小于等于当前trx_id的数据                             |
| 不可重复读 | 第一次读取数据时，记录记录的版本号，再次读取时还是读取这个版本的数据 |

新的隔离级：

| 等级       | 描述                                                                             | 解决问题             | 存在问题                                                                                                                               |
|----------|--------------------------------------------------------------------------------|------------------|------------------------------------------------------------------------------------------------------------------------------------|
| 快照隔离SI | 一个事务中所有读操作读到的数据，要么是事务一开始时的版本，要么是本事务内修改的版本 | 脏读，脏写，不可重复读 | 写倾斜：两个写事务分别更新具有一致性约束的不同列，可能会导致最终提交的数据破坏了一致性约束，这一点需要应用程序额外处理（select for update） |
| 序列化快照隔离SSI | 在SI基础上，对读写依赖进行检测，杀死可能会导致anomaly的事务，不过可能存在误杀       | 效果上等同序列化，拥有更好的性能 |

#### 并发控制

**乐观程度**：

* 基于Lock：在操作或事务开始前加锁，阻塞冲突动作
* 基于时间戳：在事务开始时获得全局递增的时间戳，按照时间戳执行事务，遇到冲突时时间戳在前的先执行
* 基于Validation：在事务提交时对冲突事务进行回滚。

#### 分布式事务

分布式时序：

1. 同时
2. 先后：因果，时间戳差距超过时钟误差

两阶段提交(保证原子性)：

1. prepare
2. commit

分布式事务模型：

### 备份恢复

#### 检查点协议

日志系统能够帮助数据库在重新启动后恢复数据库，但我们不可能每次重启都从头应用一次日志。检查点就像是游戏中的存档点一样，允许数据库忽略在检查点前的日志，以减少系统恢复的时间。

理想的检查点：

1. 不影响集群性能
2. 不需要太多计算机资源

策略1，一致的检查点：

* 大部分检查点协议的方法
* 某个时间点数据库的快照，不包含未提交的变化
* 恢复过程无需额外处理，如果需要恢复未提交事务，需要与日志文件结合处理

策略2，模糊的检查点：

* 可能包含未提交的更新
* 需要额外处理来解决未提交事务

频率：

* 太频繁会影响集群性能
* 间隔太长会导致恢复时间太长

内存与磁盘：

* 基于磁盘的数据库只需要对脏页做检查点
* 基于内存的需要对整个数据库做检查点

创建检查点的方式：

* 检查点线程扫描每个表并将数据异步写入磁盘

做快照的方法：

* Naive
    * 在内存中心的位置创建整个数据库的一致性副本，然后将内容写入磁盘
    * 做检查点期间阻塞所有事务
* Fork
    * fork 数据库的进程来做快照
    * 没有活跃事务时，子线程包含一致的检查点
    * 有活跃事务时，通过 undo log 在子线程中回滚未提交事务
    * 父进程继续事务的处理
    * 有各种各样的问题，实际运行速度较慢
* Copy-On-Update
    * 类似于MVCC
    * 检查点创建期间，事务创建新的数据副本而不是覆盖原数据


### 数据挖掘与信息检索

数据挖掘：分析大型数据库并从中找出有用的模式

数据仓库：从多个来源收集数据，建立统一的模式

### 分布式数据库

#### 技术框架

分为管理、计算、存储三个模块彼此相互通信，业务应用在数据库集群上层，物理资源层在数据库集群下层。

* 管理
    * 元数据管理
    * 安全管理
    * 资源管理
    * 任务管理
    * 分布式时钟
    * 负载均衡
    * 资源隔离
* 计算
    * 调用接口
    * 查询解析
    * 查询优化
    * 分布式事务管理
    * 并发控制
    * 动态资源分配
* 存储
    * 数据压缩
    * 数据校验
    * 数据存储
    * 数据副本

#### 技术要求

* 物理资源层
    * 支持两种以上处理器架构，处理器具有可信计算能力
    * 支持虚拟化或云环境部署
    * 网络设备具可靠、易扩展、易维护的特性，能够实时动态感知网络拓扑的变化
    * 可动态设置的黑名单机制与防火墙机制
* 计算模块
* 存储模块
    * 数据在硬件层面持久化保存
    * 多副本数据提高数据可靠性
    * 数据的物理存储方式应对业务应用透明：包括数据的分片、复制、实际存储位置
    * 数据的物理存储发生改变应对业务应用透明: 分片规则变化，存储位置变化等
    * 存储节点发生故障之后应能快速自检与恢复
    * 对于误操作的数据能够快速恢复
    * 支持数据局部/全局加密
    * 支持压缩存储
* 管理模块

#### 功能要求

* 基本功能
    * 部署灵活
        * 本地部署
            * 单节点发起在多节点上完成部署
            * 统一的可视化安装管理向导
            * 具有安装日志，并提供相应的问题排查方式
        * 云部署
            * 和本地部署的要求基本相同
    * SQL能力
        * SQL标准：参考ANSI SQL 92、99、2003标准
        * 事务：支持满足ACID特性的分布式事务
        * 数据类型：数值、字符串、二进制字符串、字符、日期/时间、Interval、bool、大对象、枚举、集合、自定义
        * 操作符：数值运算、逻辑运算、比较运算、字符串拼接、类型强转
        * 函数：常用数字类型函数、字符函数、日期/时间函数、聚合函数、类型转换、空值处理、正则表达式、安全函数、窗口分析函数
        * 条件表达式：对比条件、逻辑条件、空值条件、符合条件、模式匹配条件、区间条件、适合条件、存在条件
        * 键值约束：主键、唯一性约束
        * 分区：应支持表分区
        * 索引：支持本地索引、分区全局索引、分布式全局索引
        * 视图：支持查询定义视图
        * 字符集编码：UTF-8，GBK，GB18030
        * 序列：自增序列、全局唯一自增序列
        * 执行计划：执行计划的展示
        * 复杂查询：跨多存储节点的复杂查询，包括Join、子查询、聚合查询
        * 函数索引
        * 表空间
        * 模式管理
        * 快速恢复：对误操作的快速恢复撤回
        * 物化视图
        * 动态系统视图
        * UDF 用户自定义函数
        * 时区
        * 存储过程：变量定义，流程控制，条件判断，循环
        * 临时表
        * 公共表达式CTE（MySQL中With语句）
        * 数据分页查询
    * 读写分离：
        * 充分的读扩展能力
        * 能够提供弱一致性读（不保证读到最新数据，但读到的数据变化应当是按照时间顺序变化的）
        * 读请求能够在个数据副本间均衡分布
        * 能自动暂停数据不一致或同步延迟过大的数据副本对外提供服务，在其恢复后自动重新提供服务
        * 读请求具体的调度机制应对应用透明
    * 并发能力：
        * 并发分布式事务
        * 并发复杂查询
        * 并发连接数量限制
        * 具有一定的并行计算能力：对包括不限于跨数据分片的DDL、DML、查询语句，能够充分利用多个节点的计算能力和存储能力
        * 对各种并行计算任务，应提供控制并行度的方法
    * 适配性：
        * 软件适配
            * 兼容主流Linux操作系统
            * 不限制主流编程语言，兼容JDBC、ODBC等编程语言访问接口
            * 至少兼容一种开源数据库协议
        * 硬件适配
            * 支持至少两种硬件平台：X86、ARM、MIPS等
* 分布式特性
    * 分布式事务
        * 原子性
            * 事务提交时，事务所涉及的数据应在对应数据节点上全部修改成功；事务撤回时，所涉及数据修改应全部撤回到没有被该事务修改的状态
            * 即使在事务执行的过程中遇到异常、故障，事务原子性仍能保证不被破坏
        * 一致性
            * 实际落盘的数据一致性约束不会被破坏
            * 已提交的数据，能够实时被后续读操作访问到最新数据
            * 同一事务提交的数据，要么都能被读到，要么都不能读到
        * 隔离性：支持RC，RR，Serializable隔离级
        * 持久性
            * 已提交的事务对数据所做的修改应持久化存储，不会无故丢失和回滚，即使数据库系统发生故障或重启，数据库仍能恢复到最后一个事务成功提交时的状态。
        * 锁管理
            * 锁类型管理
            * 锁级别管理
            * 互斥管理
            * 死锁处理等
    * 服务高可用
        * 集群中全部组件都需要有高可用部署方案
        * 在发生节点级故障之后，RTO（Recovery Time Objective，复原时间目标）应该在秒级
        * 能够自动化完成节点切换，多副本场景下，主副本切换的优先级本机房、同城、异地的优先级进行选择
        * 节点扩容、数据动态分布对服务的影响应该也在秒级
    * 数据高可靠
        * 通过分布式一致性算法保证多数据副本的一致性
    * 数据安全
        * 符合数据库安全相关要求
    * 数据分片
        * 数据分片规则可灵活配置
        * 支持通用数据分片策略：Range，List，Hash
    * 弹性扩展
        * 水平扩展
            * 节点扩缩容
                * 保证上层应用的业务不需要停机维护
                * 保证事务的一致性和数据完整性
                * 能够支持同一硬件架构但型号不同的设备
                * 最好能够支持不同硬件架构的设备
            * 数据重分布
                * 在扩缩容后能够根据新的拓扑进行数据重分布
                * 保证上层应用的业务不需要停机维护，对在线业务影响尽可能少
        * 垂直扩展
            * 能够通过拓展单台服务器的硬件资源，提高集群性能
* 运维管理
    * 基本要求
        * 具有统一的图形化界面，并提供API接口，展示节点的组网关系
        * 支持SQL分析功能，展示执行计划、执行链路
        * 拥有对集群健康的评估能力，用户可以自定义健康评估指标
        * 能够记录活跃会话、集群负载、执行计划与执行次数、资源等待、节点异常等历史信息
    * 自动化部署
        * 统一的可视化安装和软件组件管理功能
        * 用户能简单地添加、修改和删除各类节点
        * 能够在部署前完成环境检查，提供合理的安装资源参考指标
    * 自动告警
        * 对系统运行的重要事件、异常事件、异常状态进行自动告警
        * 允许用户设置告警的监控项、告警阈值、告警触发条件等
        * 告警信息应能实时展示
        * 提供告警API接口
    * 状态监控
        * 对系统状态进行实时监控，包括物理服务器状态、数据库服务状态、节点间同步状态
        * 可设置监控数据的保存时间
    * 性能监控
        * CPU
        * 内存
        * 磁盘空间
        * IO
        * 网络
        * 集群状态
        * TPS、QPS
        * 慢SQL统计
        * 锁、等待事件
        * 会话连接监控
    * 备份恢复
        * 在线备份
        * 离线备份
        * 全量备份
        * 增量备份
        * 逻辑备份
        * 物理备份
        * 备份的自动化管理
        * 可自定义备份策略
        * 误操作快速恢复
        * 在不阻塞业务应用的情况下，全局的强一致备份
        * 通过备份与日志能将数据库恢复到指定时间点
        * 能显示备份进度与备份日志
        * 备份内容可压缩
        * 备份内容可加密
        * 备份恢复可选的粒度为实例级、库级、表级
        * 支持集群外服务器的恢复功能
    * 系统配置
        * 全局范围内数据库参数的在线配置
        * 安装部署时的初始化参数可配置
    * 版本升级
        * 可自动化升级
        * 可滚动升级
        * 对集群中的开源组件有更新维护的能力
        * 能够在升级前对环境进行检查
        * 升级过程可视化
        * 升级过程中能够进行人工纠错与干预
        * 能够进行版本回退
    * 系统日志
        * 能够对各类事件进行记录，完整正确
        * 提供日志的集中管理功能，能够在线查询、检索各个节点的日志
    * 导入导出
        * 表级、库级的导入导出
        * 能在数据导入导出时完成字符集的转换
        * 元数据、数据分别导入导出
        * 并行的数据导入导出
        * csv、xlsx、et、txt、log等多种格式的数据导入导出
        * 对数据文件压缩、解压缩、加密、解密
        * 导入过程中数据异常点的记录功能
    * 数据同步
        * 同构数据库同步
        * 异构数据库同步
    * 数据迁移
        * 支持至少一种主流关系型数据库的数据迁移
        * 拥有自己的迁移工具或是兼容第三方迁移工具
    * 扩缩容
        * 集群中各种组件应当都支持动态扩缩容
        * 扩缩容动作应尽可能自动化
        * 扩缩容任务中应记录操作日志，以方便问题排查与操作行为记录
        * 扩缩容过程中不影响集群对外提供服务
    * 多租户
        * 支持租户的创建、销毁、扩容、所容、迁移、备份恢复、权限设置等
        * 单个租户的规格可以超过单台物理机限制
        * 支持基于多租户的资源隔离：包括不限于CPU、内存、网络、I/O
        * 支持专属集群模式：对重要租户的数据库实例可以部署到指定专属服务器资源池中

#### 灾备要求








#### shard

将一组存储不同数据的服务器，通过联邦模式连接一起对外提供服务，被称为shard。

1. 高可用：单台服务器故障，其他服务器上的数据仍可对外提供服务
2. 更高效的查询：如果shard中的数据分散是有规律的，那么范围查询与定点查询，就可以定位到具体的一台服务器上进行。
3. 更高的写入带宽：能够多台服务器并行写入，提高写入吞吐。


#### partition

#### 分布式一致性

在单机场景下，所有读写操作都按时间顺序执行，多线程并发读到的结果是一致的。

在分布式场景下，同一数据在不同节点上的不同副本，在以整个集群为目标的并发线程下，每个线程读到的结果是很可能不一致的。

* 严格一致：所有副本都完成同步变更之后，才能被读操作读到
* 线性一致：1. 所有读写都在一个单调递增的时间线上串行地向前推进；2.所有读都能读到最近写操作的值
* 顺序一致：不同的用户将在不同的时间看到我的信息，但每个用户都以同一个顺序看到我的操作。一旦看到，这篇帖子便不会消失。如果我写了多条评论，其他人也会按顺序的看见，而非乱序。
* 串行一致：就是事务隔离级中的串行化，即读写操作所见的结果，符合某一种单机串行化所产生的结果即可。
* 因果一致：保持因果性，所见数据不一定完整，论坛回复
* 最终一致：

CAP不可能三角：我们不能构建一个完全可用的线性化系统

### 高级功能

#### 物化视图

实际存储了（物化）的视图，实际是




## 数据库测试

### 解析器

#### fuzz

sqlsmith

RQG

pquery

sqlancer

### 事务

ACID

隔离级

### 优化器

1. 结果是否正确
2. 优化是否有效果
    * 展示出的执行计划与实际执行计划的出入
    * 开关优化选项，对比性能
    * 准备不同场景下的数据集，均匀的，非均匀的
    * 高并发情况下是否性能依旧更好

https://github.com/chaos-mesh/horoscope

#### 计划类型

##### 逻辑计划


##### 物理计划

基本算子：

| 类型       | 说明                 |
|------------|--------------------|
| Scan       | 读取数据源中具体数据 |
| Selection  | 根据谓词条件过滤数据 |
| Join       | 将两个表的数据连接   |
| Projection | 向外输出数据         |

其中Scan在最底层，Projection在最顶层。

- pull 火山模型：从上到下一层一层fetch数据，将每个算子都抽象成了迭代器，具有Next方法。一种优先照顾IO效率，没有优化CPU效率的方法。

- push Pipeline：先从底层算子开始执行，算子执行结束之后，将数据推送给上层算子，通知上层算子开始执行。上下文切换比较少，对CPU cache比较友好，更适合大数据场景。

#### 衡量标准

1. 生成执行计划的质量：一条语句的正确执行计划一般都有很多种，枚举这些执行计划并根据执行效率排序，就可以得出优化器生产的执行计划具体的效率排名。
2. 生成执行计划所消耗的代价：需要多少计算机资源与时间才能得出这样的执行计划。

#### A/B test

将应用程序的两个版本相互比较来确定性能更好的版本。

流程：

1. 确定测试目标：新版本比旧版本改善的部分应该用哪些指标来评判
2. 设计测例
3. 执行测试
4. 对比结果

#### join

* 语法

| 类型                    | 描述                                                                                                                                      |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| cross join              | 没有匹配条件，完全是笛卡尔积                                                                                                               |
| （inner）join             | 根据匹配条件做笛卡尔积，求交集                                                                                                             |
| left/right (outer) join | 两表符合匹配条件的部分做笛卡尔积，不符合的部分左/又表保留所有行，另一张表为NULL                                                             |
| full (outer) join       | 两表符合匹配条件的部分做笛卡尔积，不符合的部分一张表保留所有行，另一张表为NULL，求两表并集。full join是left join 和right join 经过union的结果 |
| semi-join/anti-join     | 非标准SQL语法，主要用于查询优化器内部。只关心右表中是否存在与左表能够连接的记录，并返回相应的左表记录                                        |
| straight join           | mysql语法，能够指定以左表作为驱动表                                                                                                        |
| natural jon             | mysql 自己判断连接过程，不需要指定连接条件，mysql会使用表内相同的字段作为连接条件，有内外之分                                                |

* 实现

| 概念     | 描述                                                     |
|--------|--------------------------------------------------------|
| 驱动表   | 外层循环，从中依次取出每条记录，在内层循环中寻找相应的记录 |
| 被驱动表 | 内层循环                                                 |


| 类型             | 描述                                                                                                                  |
|------------------|---------------------------------------------------------------------------------------------------------------------|
| Nested loop join | 外层循环驱动表进行遍历，在内层循环被驱动表的过程中，找到两表匹配的行。被驱动表的匹配行上最好有顺序索引，可以提高执行效率。 |
| （Sort）Merge join|驱动表与被驱动表的匹配列上都有相同的顺序索引，根据索引找到第一个匹配行，然后同时向下遍历驱动表和被驱动表     |                                                                                                                       |
| Hash join        |选小表作为驱动表，对条件列做hash表；对被驱动表，对条件列做hash，这样分别扫描驱动表与被驱动表一次就可以找到匹配列完成join。内存不够时，可以用hash进行partition操作，逐个partition完成join操作。|



### 一致性

Jepsen

混沌测试：

| 故障类型      | 注入方法            |
|-------------|---------------------|
| 网络连接故障  | iptables            |
| 网络延迟、带宽 | tc                  |
| 停机          | kill杀进程，杀死容器 |
| 时间倾斜      |                     |
| 内核bug       |                     |

### 安全测试

#### 金融级安全要求

* 基础支撑保障
* 用户权限控制
* 数据副本一致性
* 数据副本容忍性
* 并发安全性
* 数据安全保护
* 安全审计

#### 分布式事务数据库安全体系

* 数据库外围

    * 系统安全：操作系统、虚拟化平台、开源组件等支撑性系统，要定期进行代码静态检测和行为检测，进行安全补丁更新
    * 设备安全
    * 网络安全

* 用户管理

    * 身份标识与鉴别

        * 用户口令复杂度
        * 鉴别失败之后的相应处理
        * 口令等鉴别信息的加密存储

    * 角色管理

        * 对不同数据库系统操作应提供相应的管理员角色，提供职责分离、角色约束等管理功能
        * 系统管理员（集群创建、变更、配置和维护）、审计管理员（各类日志、审计记录的查看与导出导入）、安全管理员（制定、修改、检查安全规则）三权分立的授权管理
        * 能够对普通用户进行权限限制，令其只能访问自身业务涉及到的库表数据

    * 用户授权

        * 基于用户/用户组的授权方法
        * 能够进行用户/用户组的创建、注销和登陆限制
        * 能够对用户/用户组权限进行授予与回收


* 访问控制

    * 集群访问控制
        * 多个应用访问集群时，应支持多个应用间自有数据库对象的逻辑隔离
    * 节点访问控制
        * 用户加地址的白名单
    * 对象访问控制
    * 会话访问控制
        * 不同会话拥有不同上下文
    * 资源控制
        * 并发数量限制
        * 流量控制
        * 资源保护机制，避免耽搁连接/会话消耗资源过高影响集群整体性能
    * 读写安全策略
        * 读写操作应由集群的统一入口完成，用户不能直接修改存储节点上的数据
        * 能够按权重的方式配置集群中每个节点的读写负载

* 数据安全

    * 数据副本一致性
        * 一份数据的所有副本中，一定数量的副本应当是强一致的，其他副本可以是最终一致
    * 数据副本容忍性
        * 过半数数据副本存活的情况下，能够对外提供服务
        * 不足半数数据副本存活的情况下，可以通过人工介入等手段对外提供服务
        * 副本数量可配置
        * 能够对副本存货数量进行告警，告警阈值可配置
        * 在告警阈值之内，数据应能保证不丢失
    * 数据安全恢复
        * 数据恢复时，能保证完整恢复到数据备份时刻
        * 数据恢复时，能保证数据副本全局一致
    * 剩余信息保护
        * 支持对集群扩缩容、数据重分布等过程中，产生的额外信息进行自动清理
        * 支持对数据迁移、数据导入导出过程中，对产生的中间数据进行检查清理
    * 存储安全
        * 数据表加密
        * 字段加密
        * 最好能做到加密对应用透明
    * 传输安全
        * 集群内部、外部传输过程中加密
    * 备份安全
        * 备份位置用户可选：本地备份、同城、异地
        * 备份文件加密
        * 运维接口与外部业务接口、集群内部通讯接口隔离

* 安全运维管理

    * 密钥管理
    * 监控预警
        * 硬件资源报警：CPU、内存、磁盘、IO统计、网络
        * 数据库相关报警：集群状态、集群吞吐（TPS/QPS）、SQL响应时间、慢SQL统计、锁与等待事件、会话连接
        * 提供告警API接口
    * 安全管理

* 安全审计

    * sql审计
        * 支持对所有SQL进行审计
        * 支持对对象的审计
        * 审计日志应包括：起始/完成的时间日期、连接来源的地址、连接访问的断口、事件类型、主体身份、客体目标、具体语句、事件是否成功
    * 用户行为审计
        * 用户的创建、删除、密码变更
        * 用户的权限授予与回收
        * 用户的登入登出
    * 运维审计
        * 服务启停
        * 扩缩容
        * 备份恢复



## 容器化

### Docker

docker 命令

Dockerfile

docker-compose

### k8s

k8s 体系架构

pod编排

statefulset


helm、minikube

operator
