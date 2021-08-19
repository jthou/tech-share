### 什么是redis
- Redis 全称为 Remote Dictionary Server（远程词典服务），Redis 的作者是一名意大利程序员叫 Salvatore Sanfilippo（网名：antirez）。
- Redis是一款开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存也可持久化的日志型、Key-Value高性能数据库。

### redis的特点

- 支持数据持久化，可以将内存中的数据保存在磁盘中，重启可再次加载使用
- 支持简单的Key-Value类型的数据，同时还提供List、Set、Zset、Hash等数据结构的存储
- 支持数据的备份，即Master-Slave模式的数据备份

### redis的优势

- 性能极高——Redis能支持超过 100K+ 每秒的读写频率。
- 丰富的数据类型——Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子——Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行,意思是要么成功执行要么失败完全不执行，多个操作也支持事务。

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

### redis的演进

开始 |	现在
----|---
只支持列表结构。|	支持字符串，哈希，列表，集合和有序集合等类型。
只能单机运行，没有内置的方法可以方便地将数据库分布到多台机器上。|	支持多机运行（包括复制、自动故障转移以及分布式数据库）。
少有人知的开源项目。|	广为人知并广泛使用在很多大型网站的热门开源项目。
接受捐款支持，antirez 自己无偿开发。|	Pivotal 公司出资支持开发，并且有非常多的开发者通过Github 和论坛为这个项目添砖加瓦。


### redis的使用场景
- 缓存，毫无疑问这是Redis当今最为人熟知的使用场景。再提升服务器性能方面非常有效；
- 排行榜，在使用传统的关系型数据库（mysql oracle 等）来做这个事儿，非常的麻烦，而利用Redis的SortSet(有序集合)数据结构能够简单的搞定；
- 计算器/限速器，利用Redis中原子性的自增操作，我们可以统计类似用户点赞数、用户访问数等；限速器比较典型的使用场景是限制某个用户访问某个API的频率，常用的有抢购时，防止用户疯狂点击带来不必要的压力；
- 好友关系，利用集合的一些命令，比如求交集、并集、差集等。可以方便搞定一些共同好友、共同爱好之类的功能；
- 简单消息队列，除了Redis自身的发布/订阅模式，我们也可以利用List来实现一个队列机制，比如：到货通知、邮件发送之类的需求，不需要高可靠，但是会带来非常大的DB压力，完全可以用List来完成异步解耦；
- Session共享，以PHP为例，默认Session是保存在服务器的文件中，如果是集群服务，同一个用户过来可能落在不同机器上，这就会导致用户频繁登陆；采用Redis保存Session后，无论用户落在那台机器上都能够获取到对应的Session信息。
- 一些频繁被访问的数据，经常被访问的数据如果放在关系型数据库，每次查询的开销都会很大，而放在redis中，因为redis 是放在内存中的可以很高效的访问。

### redis编译与安装 （Ubuntu）

```bash
  sudo apt install tcl -y
  wget http://download.redis.io/releases/redis-6.0.6.tar.gz
  tar xzf redis-6.0.6.tar.gz
  cd redis-6.0.6
  make
  make test
  sudo make install
```  

### redis的学习资料


### 参考
- [1][https://haicoder.net/redis/redis-birth.html](https://https://haicoder.net/redis/redis-birth.html)
- [2][https://zhuanlan.zhihu.com/p/100200635](https://zhuanlan.zhihu.com/p/100200635)
- [3][https://www.bilibili.com/video/BV1S54y1R7SB?from=search&seid=13225722623255177774](https://www.bilibili.com/video/BV1S54y1R7SB?from=search&seid=13225722623255177774)
