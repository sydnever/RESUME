# 宋源栋

13260368104 @Beijing

sydnever@foxmail.com

## 教育背景

哈尔滨工业大学 软件工程专业 本科

2018年毕业

CET-4 486分

## 工作经历

### 2019-2021

北京万里开源软件公司/北京拓林思（TurboLinux）软件公司，数据库测试部门主管

* 测试人员的招聘与培训

    * 由于人才市场中专门做过基础软件测试的人才太少，所以选择招聘一些初级DBA，往数据库测试方向培养

* 根据需求规划并进行日常测试工作

    * 迁移适配MySQL官方测例
    * 对公司内部其他数据库周边产品进行测试
    * 对公司数据库产品进行功能测试、性能测试、稳定性测试
    * 参与用户POC测试

* 从零整理构建测试体系
* 自动化测试平台的设计与研发

### 2018-2019

北京万里开源软件公司/北京拓林思（TurboLinux）软件公司，数据库研发工程师

* 混沌测试工具Jepsen的调研与对数据库中间件DBScale的适配

    * 厘清Jepsen的测试流程与测试目的，将DBScale与Jepsen进行适配

    * 改造了Jepsen原始的测试场景，将其针对DBScale的flashback功能进行测试，提高了整个产品的稳定性

* 混沌测试工具DBomb的开发与维护

    * 基于Jepsen的设计理念，设计并使用go语言开发了DBomb，便于继续开发更多测试场景

* 分布式数据库产品GreatDB5.0开发

    * 分布式事务管理器DTM调研、开发、与性能测试
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
