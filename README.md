# 宋源栋

18745684751 @Harbin

13260368104 @Beijing

sydnever@foxmail.com

https://github.com/sydnever

---
### 哈尔滨工业大学 软件工程专业 本科

2018年毕业

CET-4 486分

---
2017.07-2018.05 北京万里开源软件公司/北京拓林思（TurboLinux）软件公司，数据库开发方向，实习工程师

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
