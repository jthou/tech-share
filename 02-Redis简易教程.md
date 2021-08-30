## redis简介

### Redis诞生

> antirez 早年是系统管理员，2004 到 2006 年做嵌入式工作，之后接触 web， 2007 年和朋友共同创建一个网站 LLOOGG.com。随着 LLOOGG.com 的用户越来越多，LLOOGG.com 要维护的列表数量也越来越多，要执行的推入和弹出操作也越来越多。
>
> LLOOGG.com 当时使用 MySQL 数据库，而 MySQL 每次执行推入和弹出操作都要进行硬盘写入和读取，程序的性能严重受制于硬盘 I/O 。最终，LLOOGG.com 所使用的 MySQL 再也没办法在当时的 VPS 上处理新增的大量负载。
>
> 由于 LLOOGG.com 当时还没有找到盈利模式，所以为了尽量节约开支，antirez 没有选择直接升级 LLOOGG.com 所使用的 VPS， 而是打算另寻办法，在现有硬件的基础上， 通过提升列表操作的性能来解决负载问题。
>
> 所以 antirez 决定自己写一个具有列表结构的内存数据库原型。这个数据库原型支持 O(1) 复杂的推入和弹出操作，并且将数据储存在内存而不是硬盘，所以程序的性能不会受到硬盘 I/O 限制，可以以极快的速度执行针对列表的推入和弹出操作。
>
> 经过实验，这个原型的确可以在不升级 VPS 的前提下解决 LLOOGG.com 当时的负载问题。
>
> 后来 antirez 使用 C 语言 重写了这个内存数据库，并给它加上了持久化功能，就这样伟大的 Redis 诞生了。
>
- Redis 全称为 Remote Dictionary Server（远程词典服务）。

- Redis是一款开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存也可持久化的日志型、Key-Value高性能数据库。

### redis 可以用来干什么

根据redis的特点，我们可以用它：

- 作为数据缓存，解决MySQL读取的速度瓶颈；
- 解决并发的同步问题；
- 解决多进程的同步问题；
- 解决跨服务器进程的同步问题。

#### redis 的使用场景

- 缓存，毫无疑问这是Redis当今最为人熟知的使用场景，在提升服务器性能方面非常有效；
- 简单消息队列，可以利用List来实现一个队列机制；
- 集合运算；
- 计算器/限速器；
- 发布/订阅；
- 多服务进程之间的数据共享。


### redis 的特点

- 支持数据持久化，可以将内存中的数据保存在磁盘中，重启可再次加载使用
- 支持简单的Key-Value类型的数据，同时还提供List、Set、Zset、Hash等数据结构的存储
- 支持数据的备份，即Master-Slave模式的数据备份

### redis 的优势

- 性能极高——Redis能支持超过 100K+ 每秒的读写频率。
- 丰富的数据类型——Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子——Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行,意思是要么成功执行要么失败完全不执行，多个操作也支持事务。

### redis 的局限

- 缓存和数据库双写一致性问题
- 缓存雪崩问题
- 存击穿问题
- 缓存的并发竞争问题

## redis 安装与配置 （Ubuntu）

### 编译

```bash
  sudo apt install tcl -y
  wget http://download.redis.io/releases/redis-6.0.6.tar.gz
  tar xzf redis-6.0.6.tar.gz
  cd redis-6.0.6
  make
  make test
  sudo make install
```

### redis 配置

#### 启动 daemon 模式

```ini
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes

```

#### 持久化

```ini
################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1
save 300 10
save 60 10000

```

```ini
# The filename where to dump the DB
dbfilename dump.rdb

```

```ini
# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"

```



#### 客户端连接

### 服务启动

### 基准测试

### 备份和恢复

### 持久化

### 安全

## redis的数据类型和常用命令

### key （键）

### string （字符串）

### Hashe （哈希）

### List （列表）

redis的list实际上是一个双向链表

- 在两边插入效率会比较高
- 在中间插入的复杂度是O(n)

#### lpush/lpop/rpush/rpop

**把list用做栈或者队列**

```bash
127.0.0.1:6379[2]> lpush list one  # 左侧插入
(integer) 1
127.0.0.1:6379[2]> lpush list two
(integer) 2
127.0.0.1:6379[2]> lpush list three
(integer) 3
127.0.0.1:6379[2]> lrange list 0 -1  # 查看所有
1) "three"
2) "two"
3) "one"
127.0.0.1:6379[2]> rpush list four   # 右侧插入
(integer) 4
127.0.0.1:6379[2]> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "four"
127.0.0.1:6379[2]> lpop list # 左侧pop
"three"
127.0.0.1:6379[2]> rpop list # 右侧pop
"four"
127.0.0.1:6379[2]> lrange list 0 -1
1) "two"
2) "one"

```

#### lindex

**通过下标获取值**

```bash
127.0.0.1:6379[2]> lindex list 1
"one"

```

#### llen

**获取list的长度**

```bash
127.0.0.1:6379[2]> llen list
(integer) 2

```

#### lrem

**移除指定的值**

```bash
127.0.0.1:6379[2]>  rpush list one  
(integer) 3
127.0.0.1:6379[2]> lrange list 0 -1
1) "two"
2) "one"
3) "one"
127.0.0.1:6379[2]> lrem list 2 one # 从list中移除2个“one”
(integer) 2
127.0.0.1:6379[2]> lrange list 0 -1
1) "two"

```

#### trim

```bash
127.0.0.1:6379[2]> del list
(integer) 1
127.0.0.1:6379[2]> lrange list 0 -1
(empty array)
127.0.0.1:6379[2]> lpush list one two three four
(integer) 4
127.0.0.1:6379[2]> lrange list 0 -1
1) "four"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379[2]> ltrim list 1 4
OK
127.0.0.1:6379[2]> lrange list 0 -1
1) "three"
2) "two"
3) "one"

```

#### rpoplpush

**列表数据迁移**

```bash
127.0.0.1:6379[2]> lrange list 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379[2]> rpoplpush list list2
"one"
127.0.0.1:6379[2]> lrange list 0 -1
1) "three"
2) "two"
127.0.0.1:6379[2]> lrange list2 0 -1
1) "one"
```

#### lset

**设置指定下标的值**

```bash
# lset
# 对空列表(key 不存在)进行 LSET
redis> EXISTS list
(integer) 0
redis> LSET list 0 item
(error) ERR no such key
# 对非空列表进行 LSET
redis> LPUSH job "cook food"
(integer) 1
redis> LRANGE job 0 0
1) "cook food"
redis> LSET job 0 "play game"
OK
redis> LRANGE job  0 0
1) "play game"
# index 超出范围
redis> LLEN list                    # 列表长度为 1
(integer) 1
redis> LSET list 3 'out of range'
(error) ERR index out of range
```

#### linsert

**插入一个值**

```bash
# linsert
redis> RPUSH mylist "Hello"
(integer) 1
redis> RPUSH mylist "World"
(integer) 2
redis> LINSERT mylist BEFORE "World" "There"
(integer) 3
redis> LRANGE mylist 0 -1
1) "Hello"
2) "There"
3) "World"
# 对一个非空列表插入，查找一个不存在的 pivot
redis> LINSERT mylist BEFORE "go" "let's"
(integer) -1                                    # 失败
# 对一个空列表执行 LINSERT 命令
redis> EXISTS fake_list
(integer) 0
redis> LINSERT fake_list BEFORE "nono" "gogogog"
(integer) 0             
```

#### RPUSHX
将值 `value` 插入到列表 `key` 的表尾，当且仅当 `key` 存在并且是一个列表。

> 和 [*RPUSH*](http://doc.redisfans.com/list/rpush.html#rpush) 命令相反，当 `key` 不存在时， [RPUSHX](http://doc .redisfans.com/list/rpushx.html#rpushx) 命令什么也不做。

```bash
# key不存在
redis> LLEN greet
(integer) 0
redis> RPUSHX greet "hello"     # 对不存在的 key 进行 RPUSHX，PUSH 失败。
(integer) 0
# key 存在且是一个非空列表
redis> RPUSH greet "hi"         # 先用 RPUSH 插入一个元素
(integer) 1
redis> RPUSHX greet "hello"     # greet 现在是一个列表类型，RPUSHX 操作成功。
(integer) 2
redis> LRANGE greet 0 -1
1) "hi"
2) "hello"
```

#### 



### Set（集合）

set中的值不能重复。

#### SADD
**SADD key member [member ...]**
- 将一个或多个 `member` 元素加入到集合 `key` 当中，已经存在于集合的 `member` 元素将被忽略。
- 假如 `key` 不存在，则创建一个只包含 `member` 元素作成员的集合。
- 当 `key` 不是集合类型时，返回一个错误。
**时间复杂度:** O(N)， `N` 是被添加的元素的数量。
**返回值:** 被添加到集合中的新元素的数量，不包括被忽略的元素。

```bash
# 添加单个元素
redis> SADD bbs "discuz.net"
(integer) 1
# 添加重复元素
redis> SADD bbs "discuz.net"
(integer) 0
# 添加多个元素
redis> SADD bbs "tianya.cn" "groups.google.com"
(integer) 2
redis> SMEMBERS bbs
1) "discuz.net"
2) "groups.google.com"
3) "tianya.cn"
```

#### SCARD

返回集合 `key` 的基数(集合中元素的数量)。
**时间复杂度:**O(1)
**返回值：**集合的基数。当 `key` 不存在时，返回 `0` 。

```bash
redis> SADD tool pc printer phone
(integer) 3
redis> SCARD tool   # 非空集合
(integer) 3
redis> DEL tool
(integer) 1
redis> SCARD tool   # 空集合
(integer) 0
```
### Sorted Set

### Hash

map集合，

```
set myhash field
```



 



### HyperLogLog

### GEO

### Stream

### 事务

### 脚本

### 连接

## redis 编程

### nodejs

### python

## Redis持久化

目前，Redis 的持久化主要有两大机制，即 AOF 日志和 RDB 快照。



## Redis和MySQL

一个直观的模型是，client读写redis，redis将数据同步到MySQL，这种方案的问题在于，如果redis出现宕机，数据就会丢失。

![image-20210830144346295](./img/02-Redis%E7%AE%80%E6%98%93%E6%95%99%E7%A8%8B/image-20210830144346295.png)



> redis宕机的概率有多大?

### 缓存与数据一致性

一种比较直观的结构

业界基本一致的意见是，没有完美的数据一致性方案，只能在性能和数据准确性之间做妥协。

- 读redis，写MySQL，缓存只做失效，不做更新
- 写MysQL后通知redis对应的缓存失效，去数据库缓存新数据





### 缓存穿透

缓存穿透是指查询一个根本不存在的数据，缓存层和存储层都不会命中，但是出于容错的考虑，如果从存储层查不到数据则不写入缓存层，这样会导致所有的请求都会打向MySQL。

解决办法：当在缓存中无法命中对应数据时，且访问数据库也没有查询到目标数据，这时向缓存中存入空结果。这样的情况下，每一次查询首先判断redis中是否有目标数据（即exist(String key)），key存在就直接返回缓存结果，即使缓存结果是空值，这个方法也成为**布隆过滤器**。

![image-20210830160122814](./img/02-Redis%E7%AE%80%E6%98%93%E6%95%99%E7%A8%8B/image-20210830160122814.png)

### 缓存雪崩

redis中的存储的热点数据有一定的有效期，未来某一时刻这些数据会被redis清空，如果热点数据集体失效，那么所有的请求都会打向MySQL，轻则影响用户体验，重则导致Mysql服务器宕，该问题就是被称为缓存雪崩。

解决办法：

- 在数据库访问层面，加锁/队列式的穿行访问
- 分析系统缓存实际情况（包括用户使用场景等），设计分布较为均匀的失效时间。
- 数据预热，在能遇见的并发高峰来临前，提前均匀的、有计划的更新缓存数据，防止在并发高峰期出现缓存大量失效的情况。
- 设置业务热点数据永不过期，只做缓存更新操作。
- 在分布式数据库的情况下，将热点数据均匀分布，分散缓存雪崩后带来的单个数据库访问压力
- 访问限流（最不推荐）

加锁或者是队列式的访问控制，一定会带来性能的损耗，能提前避免的问题就尽量提前避免，最好不要等到意外发生了再做补救。

### 本节参考资料

[1]. [https://www.cnblogs.com/upnote/p/13185047.html](https://www.cnblogs.com/upnote/p/13185047.html)

[2]. [https://www.cnblogs.com/study-everyday/p/6610341.html](https://www.cnblogs.com/study-everyday/p/6610341.html)



## 应用案例

### web 服务与 AI 算法系统架构 

## redis的学习资料


### 参考
- [1] [https://haicoder.net/redis/redis-birth.html](https://https://haicoder.net/redis/redis-birth.html)
- [2] [https://zhuanlan.zhihu.com/p/100200635](https://zhuanlan.zhihu.com/p/100200635)
- [3] [https://www.bilibili.com/video/BV1S54y1R7SB?from=search&seid=13225722623255177774](https://www.bilibili.com/video/BV1S54y1R7SB?from=search&seid=13225722623255177774)
- [4] [http://doc.redisfans.com/index.html](http://doc.redisfans.com/index.html)
- [5] [https://www.zhihu.com/question/36413559](https://www.zhihu.com/question/36413559)

