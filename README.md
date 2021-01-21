# 宋源栋

13260368104 @Beijing

sydnever@foxmail.com

## 教育背景

哈尔滨工业大学 软件工程专业 本科

2018年毕业

CET-4 486分

## 职业技能与知识储备

### 编程语言

总体代码量不是很多，对大多数常用编程语言都有一定的掌握，能够在有概要设计文档的情况下，快速上手大多数项目的基础研发工作。

C/C++：编写过自研引擎一些接口相关的代码，能够一定程度上阅读理解Group Replication插件相关代码，并进行一些小范围改造。

Golang：自研过一些中小规模的数据库测试工具，为CockroachDB提交过代码。

Python：大学里学习过，现在用的比较少，快速上手没有问题。

Clojure：使用原生Jepsen的时候，写过较多代码，现在忘得差不多了。

Java：做过Benchmarksql对MySQL的适配，还做过一些简单的小工具。

Lua：为sysbench写过测试脚本。

Shell：经常写一些百行以内的工具脚本。

### 算法能力

OJ和那种算法面试题的话，很久都没刷过了。

能够阅读理解分布式领域常见的一些算法相关的材料和论文，比如分布式一致性相关算法、分布式数据库数据分布的算法、SQL优化器相关的算法等。不过没有从零实现的相关经验。

### 数据库方面





## 工作经历

### 2019-2021

北京万里开源软件公司/北京拓林思（TurboLinux）软件公司，数据库测试部门主管

* 测试人员的招聘与培训

    * 由于人才市场中专门做过基础软件测试的人才太少，所以选择招聘一些初级DBA，往数据库测试方向培养。成功培养了两名初级数据库测试，能够胜任基本的黑盒测试和性能测试。

* 根据需求规划并进行日常测试工作

    * 迁移适配MySQL官方mtr测例：将原始测例进行改写，能够同时自动化对比InnoDB与GreatDB在相同语句下的执行结果，发现Bug 100余个。
    * 对公司内部其他数据库周边产品进行测试：异构数据库数据迁移工具，RDS管理平台等。
    * 对公司数据库产品进行功能测试、性能测试、稳定性测试
    * 参与用户POC测试

* 从零整理构建公司内部测试体系（在2019年前，公司并没有专门的测试部门）
* 自动化测试平台的设计与原型研发：集成多种数据库测试工具，能够在容器或物理机环境完成数据库的部署、测试、测试结果检查。

### 2018-2019

北京万里开源软件公司/北京拓林思（TurboLinux）软件公司，数据库研发工程师

* 混沌测试工具Jepsen的调研与对数据库中间件DBScale的适配

    * 厘清Jepsen的测试流程与测试目的，将DBScale与原生Jepsen进行适配

    * 改造了Jepsen原始的测试场景，将其针对DBScale的flashback功能进行测试，提高了整个产品的稳定性

* 混沌测试工具DBomb的开发与维护

    * 基于Jepsen的设计理念，设计并使用go语言开发了DBomb，便于继续开发更多定制化测试场景

* 分布式数据库产品GreatDB5.0开发

    * 参与分布式事务管理器DTM调研、开发，进行性能测试
    * 产品雏形时期的一些基础接口的实现
    * 元数据信息统计功能的基础实现

### 2017-2018

北京万里开源软件公司/北京拓林思（TurboLinux）软件公司，数据库开发方向，实习工程师

* Oracle数据导出工具开发：使用OCI库，将数据导出成可自定义格式的CSV文件。
    * 独立完成
* 对MySQL数据库中间件DBScale，复杂语句相关功能进行增强
    * 使用Spark框架，以C++/Python混合编程方式，与原有数据库中间件产品DBScale进行桥接，主要增强了复杂的跨节点join语句性能。
    * 因pyspark在C/C++中调用时，基本无法处理并发场景，选择使用Thrift和Spark Java API对桥接部分进行替换
        * 与Spark集群桥接部分代码的编写
* 基于CockroachDB的New SQL数据库开发：
    * 对CockroachDB进行TPC-H测试
    * 与同事合作开发[sql/distsql: push down NOT NULL constraints on equality columns during planning #20100](https://github.com/cockroachdb/cockroach/issues/20100)
    * 提出并着手解决[sql: There should not be a "NOT NULL" filter on "NOT NULL" columns in optimized plan #20237](https://github.com/cockroachdb/cockroach/issues/20237)
