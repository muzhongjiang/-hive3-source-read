apache-hive-2.3.9
================

Apache Hive (TM) 数据仓库软件有助于使用 SQL 读取、写入和管理保存在分布式存储中的大型数据集。 
它建立在 Apache Hadoop (TM) 之上，提供：

* 通过 SQL 轻松访问数据的工具，从而启用数据仓储任务，例如提取/转换/加载 (ETL)、报告、和数据分析
* 一种对各种数据格式强加结构的机制
* 访问直接存储在 Apache HDFS (TM) 或其他数据存储系统（如 Apache HBase (TM) 中）的文件
* 使用 Apache Hadoop MapReduce、Apache Tez 或 Apache Spark 框架执行查询。

Getting Started
===============
- 官方文档：
  http://hive.apache.org

- Installation Instructions and a quick tutorial:
  https://cwiki.apache.org/confluence/display/Hive/GettingStarted

- A longer tutorial that covers more features of HiveQL:
  https://cwiki.apache.org/confluence/display/Hive/Tutorial

- The HiveQL Language Manual:
  https://cwiki.apache.org/confluence/display/Hive/LanguageManual


Requirements
============

Java
------

| Hive Version  | Java Version  |
| ------------- |:-------------:|
| Hive 1.2      | Java 7        |
| Hive 2.x      | Java 7        |
| Hive 3.x      | Java 8        |
| Hive 4.x      | Java 8        |


Hadoop
------

- Hadoop 1.x, 2.x
- Hadoop 3.x (Hive 3.x)


打包
====================
mvn clean package  -DskipTests -Pdist