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



## 参考

[1] [https://www.cnblogs.com/martinzhang/p/3454358.html](https://www.cnblogs.com/martinzhang/p/3454358.html)

