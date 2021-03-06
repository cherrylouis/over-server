## 一 数据的物理存储形式

数据在计算机中可以使用两种物理的存储方式：

- 文件系统形式：由操作系统来管理数据，以文件形式存储，如计算机中的各种文件，其可用性和恢复性更高，适用于更大型的数据存储，例如分布式存储
- 裸数据形式：由数据库管理数据，如常见的数据库软件 mysql。其工作于存储级，使用块 I/O 操作或者 scsi 协议，跳过了操作系统直接与磁盘交互，开销低

裸数据存储方式的有点：

- 数据以结构化形式存储，使得用户可以对数据进行复杂的操作

裸数据存储方式的缺点：

- 由于裸数据管理的整个数据，一旦发生磁盘某块区域损坏，可能会导致所有的数据无法读取。
- 分布式存储中，如果使用裸数据方式，由于数据之间有结构性关联，无法合理实现分布式

## 二 数据库概述

数据库系统是用来解决企业中的一些常见问题的：

- 持久化存储
- 优化读写
- 保证数据的有效性

当前使用的数据库，主要分为两类：

- 文档型：如 sqlite，就是一个文件，通过对文件的复制完成数据库的复制；
- 服务型：如 mysql，数据存储在物理文件中，使用终端以 tcp/ip 协议进行数据操作。

当前物理的数据库都是按照 E-R 模型进行设计的，E 表示 entry 实体，R 表示 relationship，一个实体转换为数据库中的一个表，关系描述两个实体之间的对应规则，包括

- 一对一
- 一对多
- 多对多

关系转换为数据库表中的一个列；在关系型数据库中一行就是一个对象。

ER 关系图：
![](/images/sql/00-er.png)

## 三 数据库的两大结构

- 对数据也有一些管理方式，叫做存储引擎，其包含的机制有：
  - 存储机制
  - 索引方式
  - 锁
  - 等等
- 数据库管理系统(Database Management System DBMS )，专门用于管理和维护数据库所使用的软件
  - 用户管理
  - 处理数据库连接
  - 缓存
  - 查询
  - 日志
  - 等等

## 四 sql 语言

sql 语言：即结构化查询语言，是关系行数据库默认的操作语言，拥有一套标准的 sql 标准，如下所示：

- DDL：数据定义语言，DROP,CREATE,ALTER
- DML:数据操作语言，INSERT,UPDATE,DELETE
- DQL:数据查询语言，SELECT
- DCL:数据控制语言，GRANT,REVOKE,COMMIT

## 五 三大范式

三范式：设计数据库的一些规范被称为范式，后一个范式，都是在前一个范式的基础上建立的。

- 第一范式（1NF)：列不可拆分
- 第二范式（2NF)：唯一标识
- 第三范式（3NF)：引用主键

一个数据库就是一个完整的业务单元，可以包含多张表，数据被存储在表中。在表中为了更加准确的存储数据，保证数据的正确有效，可以在创建表的时候，为表添加一些强制性的验证，包括数据字段的类型、约束。

## 一 SQL 与 NoSQL

关系型数据库是建立在关系模型基础上的数据库。

关系模型的构成：

- 关系数据结构
- 关系操作集合（增删改查、join 的等）
- 关系完整的约束性（实体完整性、参照完整性、用工定义完整性）。

关系型数据库面临的问题：

- 海量数据存储和访问：如何高效率的存储和访问海量数据
- 高并发高负载下，优化数据库的成本很高

NoSQL，全名为 Not Only SQL，指的是非关系型的数据库。随着访问量的上升，网站的数据库性能出现了问题，于是 nosql 被设计出来。

非结构型数据库没有行、列的概念，用 JSON 来存储数据。集合就相当于“表”，文档就相当于“行”。有些系统，特别需要筛选，如筛选所有大约 20 岁的女生，那么 mysql 擅长，但是站内信只需要存储就好了，不需要筛选。

关系型数据库采用的结构化的数据，NoSQL 采用的是键值对的方式存储数据。

## 二 关系型数据库

### 2.1 常见关系型数据库

### 2.2 SQL 语句

关系型数据库使用 SQL（结构化查询语言 Stucture Query Language）查询数据。

SQL 是美国国家标准局于 20 世界 80 年代订立的数据库语言标准，目前大部分关系型数据库都支持 SQL 语言。  
SQL 语句分类：

- DDL：Data Definition Languages，定义了数据库对象的操作，如 create、drop、alter
- DML：Data Manipulation Language，定义了数据操作语句，如增删改查
- DCL：Data Control Language，定义了数据访问级别，如数据库、表、字段、用户的访问权限，有 grant、revoke 语句

## 三 非关系型数据库

### 3.1 常见非关系型数据库

| 类型       | 常见数据库名        | 特点                                                                                                         |
| ---------- | ------------------- | ------------------------------------------------------------------------------------------------------------ |
| 列存储     | Hbase Cassandra     | 按列存储数据的，方便存储结构化和半结构化数据，方便做数据压缩，对针对某一列或者某几列的查询有非常大的 IO 优势 |
| 文档存储   | MongoDB CouchDB     | 按列存储数据的，方便存储结构化和半结构化数据，方便做数据压缩，对针对某一列或者某几列的查询有非常大的 IO 优势 |
| k-v 存储   | MemcacheDB Redis    | 可以通过 key 快速查询到其 value。一般来说，存储不管 value 的格式，照单全收。（Redis 包含了其他功能）         |
| 图存储     | Neo4J FlockDB       | 图形关系的最佳存储。使用传统关系数据库来解决的话性能低下，而且设计使用不方便。                               |
| 对象存储   | db4o Versant        | 文通过类似面向对象语言的语法操作数据库，通过对象的方式存取数据。                                             |
| xml 数据库 | BerkeleyDBXML BaseX | 高效的存储 XML 数据，并支持 XML 的内部查询语法，比如 XQuery,Xpath。                                          |

### 3.2 NoSQL 优缺点

NoSQL 优点:

- 高可扩展性
- 分布式计算
- 低成本
- 架构的灵活性，半结构化数据
- 没有复杂的关系

NoSQL 缺点:

- 没有标准化
- 有限的查询功能（到目前为止）
- 最终一致是不直观的程序

## 四 SQL 与 NoSQL 选型

NoSQL 使用场景:

- 在处理非结构化/半结构化的大数据时；
- 在水平方向上进行扩展时；
- 随时应对动态增加的数据项时可以优先考虑使用 NoSQL 数据库。

在考虑数据库的成熟度、分析和商业智能、管理及专业性等问题时，应优先考虑关系型数据库。

总结 NoSQL 的使用场景：

- 1、数据模型比较简单；
- 2、需要灵活性更强的 IT 系统；
- 3、对数据库性能要求较高；
- 4、不需要高度的数据一致性；
- 5、对于给定 key，比较容易映射复杂值的环境。
