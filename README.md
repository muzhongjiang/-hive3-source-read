# apache-hive-2.3.9

Apache Hive (TM) 数据仓库软件有助于使用 SQL 读取、写入和管理保存在分布式存储中的大型数据集。 它建立在 Apache Hadoop (TM) 之上，提供：

* 通过 SQL 轻松访问数据的工具，从而启用数据仓储任务，例如提取/转换/加载 (ETL)、报告、和数据分析
* 一种对各种数据格式强加结构的机制
* 访问直接存储在 Apache HDFS (TM) 或其他数据存储系统（如 Apache HBase (TM) 中）的文件
* 使用 Apache Hadoop MapReduce、Apache Tez 或 Apache Spark 框架执行查询。

## 参考：
[编译](https://blog.csdn.net/u013289115/article/details/112464043)
[开发文档](https://cwiki.apache.org/confluence/display/Hive/DeveloperDocs)


# Getting Started

- 官方文档：
  http://hive.apache.org

- Installation Instructions and a quick tutorial:
  https://cwiki.apache.org/confluence/display/Hive/GettingStarted

- A longer tutorial that covers more features of HiveQL:
  https://cwiki.apache.org/confluence/display/Hive/Tutorial

- The HiveQL Language Manual:
  https://cwiki.apache.org/confluence/display/Hive/LanguageManual

# Requirements

## Java

| Hive Version  | Java Version  |
| ------------- |:-------------:|
| Hive 1.2      | Java 7        |
| Hive 2.x      | Java 7        |
| Hive 3.x      | Java 8        |
| Hive 4.x      | Java 8        |

## Hadoop

- Hadoop 2.x, 2.x
- Hadoop 3.x (Hive 3.x)

## 打包

mvn clean package -DskipTests -Pdist

# 本地调试：

说明：本地调试和运行hive-sql不需要依赖和安装hadoop，表数据文件存放本地系统目录，存储不依赖hdfs、计算也不依赖yarn

## thrift

由于Hive的HiveServer2和Metastore组件本质上都是一个Thrift Server，可以满足跨语言间的RPC通信。 注意:Thrift有严格的版本要求，环境的版本需要与代码里保持一致,在pom.xml文件 <
libthrift.version>标识即版本。

https://www.cnblogs.com/muzhongjiang/p/15794135.html

## protobuf

安装对应protobuf。 我本地以及安装了protobuf 3.19.3 ，所以修改了pom.xml: protobuf.version

```xml
    <!--<protobuf.version>2.5.0</protobuf.version>-->
<protobuf.version>3.19.3</protobuf.version>
```

## 修改配置文件hive-site.xml

./hive2-source-read/conf/hive-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://tencent:3306/hive?createDatabaseIfNotExist=true</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value><![CDATA[root&password@168]]></value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
        <name>datanucleus.schema.autocreateall</name>
        <value>false</value>
        <description>如果不存在，则在启动时创建必要的schema。创建一次后将此设置为false</description>
    </property>
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>file:///Users/muzhongjiang/storage/data/hive/warehouse</value>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>file:///Users/muzhongjiang/storage/data/hive</value>
    </property>
    <property>
        <name>hive.security.authorization.manager</name>
        <value>org.apache.hadoop.hive.ql.security.authorization.DefaultHiveMetastoreAuthorizationProvider</value>
        <description>The hive client authorization manager class name.</description>
    </property>
    <property>
        <name>hive.metastore.uris</name>
        <value>thrift://127.0.0.1:9083</value>
    </property>
    <property>
        <name>hive.privilege.synchronizer</name>
        <value>false</value>
    </property>
</configuration>
```

## 修改pom.xml文件

### vim pom.xml

把上面thrift的安装路径填上去

```xml

<thrift.home>/usr/local/thrift</thrift.home>
```

### vim cli/pom.xml

1、把hive-site.xml等配置文件打包

```xml

<build>
    <resources>
        <resource>
            <directory>../conf</directory>
            <includes>
                <include>*.*</include>
            </includes>
        </resource>
    </resources>
</build>
```

2、把 <scope>test</scope> 改成 <scope>runtime</scope>

```xml

<dependencies>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>${commons-io.version}</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>com.lmax</groupId>
        <artifactId>disruptor</artifactId>
        <version>${disruptor.version}</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

### vim metastore/pom.xml

1、把hive-site.xml等配置文件打包

```xml

<build>
    <resources>
        <resource>
            <directory>../conf</directory>
            <includes>
                <include>*.*</include>
            </includes>
        </resource>
    </resources>
</build>
```

2、把 <scope>test</scope> 改成 <scope>runtime</scope>

```xml

<dependency>
    <groupId>com.lmax</groupId>
    <artifactId>disruptor</artifactId>
    <version>${disruptor.version}</version>
    <scope>runtime</scope>
</dependency>
```

3、添加mysql jdbc驱动包依赖，我这里用的是mysql8

```xml

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.21</version>
</dependency>
```

### vim service/pom.xml

1、hive-site.xml等配置文件打包

```xml

<build>
    <resources>
        <resource>
            <directory>../conf</directory>
            <includes>
                <include>*.*</include>
            </includes>
        </resource>
    </resources>
</build>
```

### vim beeline/pom.xml

1、把 <scope>test</scope> 改成 <scope>runtime</scope>

```xml

<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-mapreduce-client-core</artifactId>
    <version>${hadoop.version}</version>
    <scope>runtime</scope>
</dependency>
```

## 修改HiveConf.java文件，修改参数默认值 (后面使用传参方式来修改更好)

hive.in.test=true hive.in.tez.test=true hive.exec.mode.local.auto=true

## 修改./metastore/if/hive_metastore.thrift文件第25行


include "/usr/local/thrift/thrift-0.14.1.src/contrib/fb303/if/fb303.thrift"

## 初始化元数据库

1、将"CREATE TABLE" 替换为："CREATE TABLE IF NOT EXISTS"
2、进入mysql命令行： mysql -htencent -uroot -p'root&password@168'

```mysql
CREATE database IF NOT EXISTS hive;
use hive;
SOURCE /Users/muzhongjiang/storage/git/github/Hive/hive2-source-read/metastore/scripts/upgrade/mysql/hive-schema-2.3.0.mysql.sql ;
```

TIP："hive-schema-2.3.0.mysql.sql" 内部会执行 "hive-txn-schema-2.3.0.mysql.sql"脚本。

## 编译

mvn clean package -DskipTests -Pdist -X

mvn clean compile -Dmaven.test.skip=true -P thrift,protobuf -X





























