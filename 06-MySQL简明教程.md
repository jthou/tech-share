## MySQL的引擎

Mysql有很多的表引擎，其中最常用的是MyISAM和InnoDB。

| 引擎名称 | 是否支持事务 | 优点                         | 适用场景                           |
| -------- | ------------ | ---------------------------- | ---------------------------------- |
| MyISAM   | 不支持       | 提供高速存储、检索、全文搜索 | 大量且查询频繁的环境               |
| InnoDB   | 支持         | 提供事务操作、行级锁定等     | OLTP（联机事务处理）、增改业务复杂 |

### 单表引擎修改

#### 查询数据库支持的引擎及默认设置

```sql
mysql> show engines;
```

![](./img/06-MySQL%E7%AE%80%E6%98%8E%E6%95%99%E7%A8%8B/image-20210830113333384.png)



```sql
mysql> show variables like '%storage_engine%';
```

![image-20210830113748104](img/06-MySQL%E7%AE%80%E6%98%8E%E6%95%99%E7%A8%8B/image-20210830113748104.png)



#### 查询具体某个表使用的引擎

```sql
 mysql>show create table user_table;
```

![image-20210830114318422](img/06-MySQL%E7%AE%80%E6%98%8E%E6%95%99%E7%A8%8B/image-20210830114318422.png)

#### 修改表引擎

```sql
mysql>alter table user_table engine=InnoDB;
```



## BinLog

###  binlog 基本认识

MySQL的二进制日志可以说是MySQL最重要的日志了，它记录了所有的DDL和DML(除了数据查询语句)语句，以事件形式记录，还包含语句所执行的消耗的时间，MySQL的二进制日志是事务安全型的。

两个最重要的使用场景: 

- 主从数据一致性，Mster把它的二进制日志传递给slaves来达到master-slave数据一致的目的。 
- 数据恢复，通过使用mysqlbinlog工具来使恢复数据。

### 开启binlog



## MySQL性能优化

### MySQL性能测试

性能指标：

- TPS ：Transactions Per Second ，即数据库每秒执行的事务数，以 commit 成功次数为准。
- QPS ：Queries Per Second ，即数据库每秒执行的 SQL 数（含 insert、select、update、delete 等）。
- RT ：Response Time ，响应时间。包括平均响应时间、最小响应时间、最大响应时间、每个响应时间的查询占比。比较需要重点关注的是，前 95-99% 的最大响应时间。因为它决定了大多数情况下的短板。
- Concurrency Threads ：并发量，每秒可处理的查询请求的数量。

总结来说，实际就是 2 个维度：

- 吞吐量
- 延迟

MySQL 的性能测试工具还是比较多的，使用最多的是 sysbench 和 mysqlslap 。

### 本章参考文献

1. https://www.cnblogs.com/caicairui/p/7350698.html
2. https://www.cnblogs.com/wangzhuxing/p/5223881.html
3. https://blog.csdn.net/weixin_39715997/article/details/113209768
4. https://cloud.tencent.com/developer/article/1596673
5. https://cloud.tencent.com/developer/article/1536046
6. [详解MySQL基准测试和sysbench工具 - 编程迷思 - 博客园 (cnblogs.com)](https://www.cnblogs.com/kismetv/p/7615738.html)
7. [【性能测试】MySQL数据库性能测试_三桨鱼的博客-CSDN博客](https://blog.csdn.net/qingdaoyin/article/details/120306452)
8. [测试 MySQL 性能的几款工具 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/67553416)

## 参考

[1] [https://www.cnblogs.com/martinzhang/p/3454358.html](https://www.cnblogs.com/martinzhang/p/3454358.html)

