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

#### 客户端连接

### 服务启动

### 基准测试

### 备份和恢复

### 持久化

### 安全

## redis的数据类型和常用命令

### key

### string

### Hashe

### List

### Set

### Sorted Set

### HyperLogLog

### GEO

### Stream

### 事务

### 脚本

### 连接

## redis 编程

### nodejs

### python

## 应用案例

### web 服务与 AI 算法系统架构 

## redis的学习资料


### 参考
- [1][https://haicoder.net/redis/redis-birth.html](https://https://haicoder.net/redis/redis-birth.html)
- [2][https://zhuanlan.zhihu.com/p/100200635](https://zhuanlan.zhihu.com/p/100200635)
- [3][https://www.bilibili.com/video/BV1S54y1R7SB?from=search&seid=13225722623255177774](https://www.bilibili.com/video/BV1S54y1R7SB?from=search&seid=13225722623255177774)
