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

### 主从复制

- Redis 使用异步复制。从 Redis 2.8 开始， 从服务器会以每秒一次的频率向主服务器报告复制流（replication stream）的处理进度。
- 一个主服务器可以有多个从服务器，从服务器也可以有自己的从服务器， 多个从服务器之间可以构成一个图状结构。
- 复制功能不会阻塞主服务器，也不会阻塞从服务器。
- 在从服务器删除旧版本数据集并载入新版本数据集的那段时间内， 连接请求会被阻塞。
- 可以配置从服务器， 让它在与主服务器之间的连接断开时， 向客户端发送一个错误。
- 可以通过复制功能来让主服务器免于执行持久化操作： 只要关闭主服务器的持久化功能， 然后由从服务器去执行持久化操作即可。

1， 在配置文件中加上

```ini
slaveof 192.168.1.1 6379
```

2，用命令

```bash
127.0.0.1:6379> SLAVEOF 192.168.1.1 10086
OK
```



## redis的数据类型和常用命令

### Key 

#### keys/exists/type

```bash
### keys
redis> MSET one 1 two 2 three 3 four 4  # 一次设置 4 个 key
OK
redis> KEYS *o*
1) "four"
2) "two"
3) "one"
redis> KEYS t??
1) "two"
redis> KEYS t[w]*
1) "two"
redis> KEYS *  # 匹配数据库内所有 key
1) "four"
2) "three"
3) "two"
4) "one"

### exists
redis> SET db "redis"
OK
redis> EXISTS db
(integer) 1
redis> DEL db
(integer) 1
redis> EXISTS db
(integer) 0

### type
# 字符串
redis> SET weather "sunny"
OK
redis> TYPE weather
string

# 列表
redis> LPUSH book_list "programming in scala"
(integer) 1
redis> TYPE book_list
list

# 集合
redis> SADD pat "dog"
(integer) 1
redis> TYPE pat
set
```

> KEYS的速度非常快，但在一个大的数据库中使用它仍然可能造成性能问题，如果你需要从一个数据集中查找特定的 `key` ，你最好还是用 Redis 的集合结构(set)来代替。

#### dump/restore

```bash
# 序列化给定 key ，并返回被序列化的值，使用 RESTORE 命令可以将这个值反序列化为 Redis 键。
redis> SET greeting "hello, dumping world!"
OK
redis> DUMP greeting
"\x00\x15hello, dumping world!\x06\x00E\xa0Z\x82\xd8r\xc1\xde"

#如果 key 不存在，那么返回 nil 。
redis> DUMP not-exists-key
(nil)

# RESTORE key ttl serialized-value 
# 参数 ttl 以毫秒为单位为 key 设置生存时间；如果 ttl 为 0 ，那么不设置生存时间。
redis> RESTORE greeting-again 0 "\x00\x15hello, dumping world!\x06\x00E\xa0Z\x82\xd8r\xc1\xde"
OK
redis> GET greeting-again
"hello, dumping world!"

# 使用错误的值进行反序列化
redis> RESTORE fake-message 0 "hello moto moto blah blah"  
(error) ERR DUMP payload version or checksum are wrong
```

#### expire/expireat/ttl/pexpire/pexpireat/pttl/persist

```bash
### expire
redis> SET cache_page "www.google.com"
OK
redis> EXPIRE cache_page 30  # 设置过期时间为 30 秒
(integer) 1
redis> TTL cache_page    # 查看剩余生存时间
(integer) 23
redis> EXPIRE cache_page 30000   # 更新过期时间
(integer) 1
redis> TTL cache_page
(integer) 29996

### expireat
#用于为 key 设置生存时间, 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。
redis> SET cache www.google.com
OK
redis> EXPIREAT cache 1355292000     # 这个 key 将在 2012.12.12 过期
(integer) 1
redis> TTL cache
(integer) 45081860

### pexpire
redis> SET mykey "Hello"
OK
redis> PEXPIRE mykey 1500
(integer) 1
redis> TTL mykey    # TTL 的返回值以秒为单位
(integer) 2
redis> PTTL mykey   # PTTL 可以给出准确的毫秒数
(integer) 1499

### pexpireat
redis> SET mykey "Hello"
OK
redis> PEXPIREAT mykey 1555555555005
(integer) 1
redis> TTL mykey           # TTL 返回秒
(integer) 223157079
redis> PTTL mykey          # PTTL 返回毫秒
(integer) 223157079318

### persist
# 移除给定 key 的生存时间，将这个 key 从『易失的』(带生存时间 key )转换成『持久的』
redis> SET mykey "Hello"
OK
redis> EXPIRE mykey 10  # 为 key 设置生存时间
(integer) 1
redis> TTL mykey
(integer) 10
redis> PERSIST mykey    # 移除 key 的生存时间
(integer) 1
redis> TTL mykey
(integer) -1
```

#### del/move/rename/nenamenx

```bash
### del
#  删除单个 key
redis> SET name huangz
OK
redis> DEL name
(integer) 1

# 删除一个不存在的 key
redis> EXISTS phone
(integer) 0
redis> DEL phone # 失败，没有 key 被删除
(integer) 0

# 同时删除多个 key
redis> SET name "redis"
OK
redis> SET type "key-value store"
OK
redis> SET website "redis.com"
OK
redis> DEL name type website
(integer) 3

### move
# key 存在于当前数据库
redis> SELECT 0                             # redis默认使用数据库 0，为了清晰起见，这里再显式指定一次。
OK
redis> SET song "secret base - Zone"
OK
redis> MOVE song 1                          # 将 song 移动到数据库 1
(integer) 1
redis> EXISTS song                          # song 已经被移走
(integer) 0
redis> SELECT 1                             # 使用数据库 1
OK
redis:1> EXISTS song                        # 证实 song 被移到了数据库 1 (注意命令提示符变成了"redis:1"，表明正在使用数据库 1)
(integer) 1

# 当 key 不存在的时候
redis:1> EXISTS fake_key
(integer) 0
redis:1> MOVE fake_key 0                    # 试图从数据库 1 移动一个不存在的 key 到数据库 0，失败
(integer) 0
redis:1> select 0                           # 使用数据库0
OK
redis> EXISTS fake_key                      # 证实 fake_key 不存在
(integer) 0

# 当源数据库和目标数据库有相同的 key 时
redis> SELECT 0                             # 使用数据库0
OK
redis> SET favorite_fruit "banana"
OK
redis> SELECT 1                             # 使用数据库1
OK
redis:1> SET favorite_fruit "apple"
OK
redis:1> SELECT 0                           # 使用数据库0，并试图将 favorite_fruit 移动到数据库 1
OK
redis> MOVE favorite_fruit 1                # 因为两个数据库有相同的 key，MOVE 失败
(integer) 0
redis> GET favorite_fruit                   # 数据库 0 的 favorite_fruit 没变
"banana"
redis> SELECT 1
OK
redis:1> GET favorite_fruit                 # 数据库 1 的 favorite_fruit 也是
"apple"

### rename
# key 存在且 newkey 不存在
redis> SET message "hello world"
OK
redis> RENAME message greeting
OK
redis> EXISTS message               # message 不复存在
(integer) 0
redis> EXISTS greeting              # greeting 取而代之
(integer) 1

# 当 key 不存在时，返回错误
redis> RENAME fake_key never_exists
(error) ERR no such key

# newkey 已存在时， RENAME 会覆盖旧 newkey
redis> SET pc "lenovo"
OK
redis> SET personal_computer "dell"
OK
redis> RENAME pc personal_computer
OK
redis> GET pc
(nil)
redis:1> GET personal_computer      # 原来的值 dell 被覆盖了
"lenovo"

### renamenx
# newkey 不存在，改名成功
redis> SET player "MPlyaer"
OK
redis> EXISTS best_player
(integer) 0

redis> RENAMENX player best_player
(integer) 1

# newkey存在时，失败
redis> SET animal "bear"
OK
redis> SET favorite_animal "butterfly"
OK
redis> RENAMENX animal favorite_animal
(integer) 0
redis> get animal
"bear"
redis> get favorite_animal
"butterfly"
```

#### sort

```bash
### 一般 SORT 用法
redis> LPUSH today_cost 30 1.5 10 8
(integer) 4

# 默认为正序排序
redis> SORT today_cost
1) "1.5"
2) "8"
3) "10"
4) "30"

# 逆序排序
redis 127.0.0.1:6379> SORT today_cost DESC
1) "30"
2) "10"
3) "8"
4) "1.5"

### 使用 ALPHA 修饰符对字符串进行排序
redis> LPUSH website "www.reddit.com"
(integer) 1
redis> LPUSH website "www.slashdot.com"
(integer) 2
redis> LPUSH website "www.infoq.com"
(integer) 3

# 默认（按数字）排序
redis> SORT website
1) "www.infoq.com"
2) "www.slashdot.com"
3) "www.reddit.com"

# 按字符排序
redis> SORT website ALPHA
1) "www.infoq.com"
2) "www.reddit.com"
3) "www.slashdot.com"

### 使用 LIMIT 修饰符限制返回结果， offset 指定要跳过的元素数量。count 指定跳过 offset 个指定的元素之后，要返回多少个对象。
# 添加测试数据，列表值为 1 指 10
redis 127.0.0.1:6379> RPUSH rank 1 3 5 7 9
(integer) 5
redis 127.0.0.1:6379> RPUSH rank 2 4 6 8 10
(integer) 10

# 返回列表中最小的 5 个值
redis 127.0.0.1:6379> SORT rank LIMIT 0 5
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"

# 返回从大到小排序的前 5 个对象
redis 127.0.0.1:6379> SORT rank LIMIT 0 5 DESC
1) "10"
2) "9"
3) "8"
4) "7"
5) "6"

### 使用外部 key 进行排序
# admin
redis 127.0.0.1:6379> LPUSH uid 1
(integer) 1
redis 127.0.0.1:6379> SET user_name_1 admin
OK
redis 127.0.0.1:6379> SET user_level_1 9999
OK

# jack
redis 127.0.0.1:6379> LPUSH uid 2
(integer) 2
redis 127.0.0.1:6379> SET user_name_2 jack
OK
redis 127.0.0.1:6379> SET user_level_2 10
OK

# peter
redis 127.0.0.1:6379> LPUSH uid 3
(integer) 3
redis 127.0.0.1:6379> SET user_name_3 peter
OK
redis 127.0.0.1:6379> SET user_level_3 25
OK

# mary
redis 127.0.0.1:6379> LPUSH uid 4
(integer) 4
redis 127.0.0.1:6379> SET user_name_4 mary
OK
redis 127.0.0.1:6379> SET user_level_4 70
OK

# 按uid排序
redis 127.0.0.1:6379> SORT uid
1) "1"      # admin
2) "2"      # jack
3) "3"      # peter
4) "4"      # mary

# 按user_level排序
redis 127.0.0.1:6379> SORT uid BY user_level_*
1) "2"      # jack , level = 10
2) "3"      # peter, level = 25
3) "4"      # mary, level = 70
4) "1"      # admin, level = 9999

# 先排序 uid ， 再取出键 user_name_{uid} 的值：
redis 127.0.0.1:6379> SORT uid GET user_name_*
1) "admin"
2) "jack"
3) "peter"
4) "mary"

# 先按 user_level_{uid} 来排序 uid 列表， 再取出相应的 user_name_{uid} 的值：
redis 127.0.0.1:6379> SORT uid BY user_level_* GET user_name_*
1) "jack"       # level = 10
2) "peter"      # level = 25
3) "mary"       # level = 70
4) "admin"      # level = 9999

# 按 uid 分别获取 user_level_{uid} 和 user_name_{uid} ：
redis 127.0.0.1:6379> SORT uid GET user_level_* GET user_name_*
1) "9999"       # level
2) "admin"      # name
3) "10"
4) "jack"
5) "25"
6) "peter"
7) "70"
8) "mary"

### GET 有一个额外的参数规则，那就是 —— 可以用 # 获取被排序键的值。
### 以下代码就将 uid 的值、及其相应的 user_level_* 和 user_name_* 都返回为结果：
redis 127.0.0.1:6379> SORT uid GET # GET user_level_* GET user_name_*
1) "1"          # uid
2) "9999"       # level
3) "admin"      # name
4) "2"
5) "10"
6) "jack"
7) "3"
8) "25"
9) "peter"
10) "4"
11) "70"
12) "mary"

### 获取外部键，但不进行排序
redis 127.0.0.1:6379> SORT uid BY not-exists-key
1) "4"
2) "3"
3) "2"
4) "1"

### 在不排序的情况下， 获取多个外部键， 相当于执行一个整合的获取操作（类似于 SQL 数据库的 join 关键字）。
redis 127.0.0.1:6379> SORT uid BY not-exists-key GET # GET user_level_* GET user_name_*
1) "4"      # id
2) "70"     # level
3) "mary"   # name
4) "3"
5) "25"
6) "peter"
7) "2"
8) "10"
9) "jack"
10) "1"
11) "9999"
12) "admin"

### 将哈希表作为 GET 或 BY 的参数
redis 127.0.0.1:6379> HMSET user_info_1 name admin level 9999
OK
redis 127.0.0.1:6379> HMSET user_info_2 name jack level 10
OK
redis 127.0.0.1:6379> HMSET user_info_3 name peter level 25
OK
redis 127.0.0.1:6379> HMSET user_info_4 name mary level 70
OK

redis 127.0.0.1:6379> SORT uid BY user_info_*->level
1) "2"
2) "3"
3) "4"
4) "1"
redis 127.0.0.1:6379> SORT uid BY user_info_*->level GET user_info_*->name
1) "jack"
2) "peter"
3) "mary"
4) "admin"

### 保存排序结果
# 测试数据
redis 127.0.0.1:6379> RPUSH numbers 1 3 5 7 9
(integer) 5
redis 127.0.0.1:6379> RPUSH numbers 2 4 6 8 10
(integer) 10
redis 127.0.0.1:6379> LRANGE numbers 0 -1
1) "1"
2) "3"
3) "5"
4) "7"
5) "9"
6) "2"
7) "4"
8) "6"
9) "8"
10) "10"
redis 127.0.0.1:6379> SORT numbers STORE sorted-numbers
(integer) 10

# 排序后的结果
redis 127.0.0.1:6379> LRANGE sorted-numbers 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
8) "8"
9) "9"
10) "10"
```



### String

#### set/setnx/get/strlen

```bash
# 对不存在的键进行设置
redis 127.0.0.1:6379> SET key "value"
OK
redis 127.0.0.1:6379> GET key
"value"

# 对已存在的键进行设置
redis 127.0.0.1:6379> SET key "new-value"
OK
redis 127.0.0.1:6379> GET key
"new-value"

# 使用 EX 选项
redis 127.0.0.1:6379> SET key-with-expire-time "hello" EX 10086
OK
redis 127.0.0.1:6379> GET key-with-expire-time
"hello"
redis 127.0.0.1:6379> TTL key-with-expire-time
(integer) 10069

# 使用 PX 选项, PX=expire
redis 127.0.0.1:6379> SET key-with-pexpire-time "moto" PX 123321
OK
redis 127.0.0.1:6379> GET key-with-pexpire-time
"moto"
redis 127.0.0.1:6379> PTTL key-with-pexpire-time
(integer) 111939

# setnx
redis> EXISTS job                # job 不存在
(integer) 0
redis> SETNX job "programmer"    # job 设置成功
(integer) 1
redis> SETNX job "code-farmer"   # 尝试覆盖 job ，失败
(integer) 0
redis> GET job                   # 没有被覆盖
"programmer"

# 使用 NX 选项, NX = if not exists. 
redis 127.0.0.1:6379> SET not-exists-key "value" NX
OK      # 键不存在，设置成功
redis 127.0.0.1:6379> GET not-exists-key
"value"
redis 127.0.0.1:6379> SET not-exists-key "new-value" NX
(nil)   # 键已经存在，设置失败
redis 127.0.0.1:6379> GEt not-exists-key
"value" # 维持原值不变

# 使用 XX 选项, XX=exists
redis 127.0.0.1:6379> EXISTS exists-key
(integer) 0
redis 127.0.0.1:6379> SET exists-key "value" XX
(nil)   # 因为键不存在，设置失败
redis 127.0.0.1:6379> SET exists-key "value"
OK      # 先给键设置一个值
redis 127.0.0.1:6379> SET exists-key "new-value" XX
OK      # 设置新值成功
redis 127.0.0.1:6379> GET exists-key
"new-value"

# NX 或 XX 可以和 EX 或者 PX 组合使用
redis 127.0.0.1:6379> SET key-with-expire-and-NX "hello" EX 10086 NX
OK
redis 127.0.0.1:6379> GET key-with-expire-and-NX
"hello"
redis 127.0.0.1:6379> TTL key-with-expire-and-NX
(integer) 10063
redis 127.0.0.1:6379> SET key-with-pexpire-and-XX "old value"
OK
redis 127.0.0.1:6379> SET key-with-pexpire-and-XX "new value" PX 123321
OK
redis 127.0.0.1:6379> GET key-with-pexpire-and-XX
"new value"
redis 127.0.0.1:6379> PTTL key-with-pexpire-and-XX # pttr, 当 key 不存在时，返回 -2, 当 key 存在但没有设置剩余生存时间时，返回 -1, 否则，以毫秒为单位，返回 key 的剩余生存时间。
(integer) 112999

# EX 和 PX 可以同时出现，但后面给出的选项会覆盖前面给出的选项
redis 127.0.0.1:6379> SET key "value" EX 1000 PX 5000000
OK
redis 127.0.0.1:6379> TTL key
(integer) 4993  # 这是 PX 参数设置的值
redis 127.0.0.1:6379> SET another-key "value" PX 5000000 EX 1000
OK
redis 127.0.0.1:6379> TTL another-key
(integer) 997   # 这是 EX 参数设置的值

### strlen
# 获取字符串的长度
redis> SET mykey "Hello world"
OK
redis> STRLEN mykey
(integer) 11

# 不存在的 key 长度为 0
redis> STRLEN nonexisting
(integer) 0
```

#### mset/mget/msetnx/setex/psetex/setrange/getrange

```bash
redis> MSET date "2012.3.30" time "11:00 a.m." weather "sunny"
OK
redis> MGET date time weather
1) "2012.3.30"
2) "11:00 a.m."
3) "sunny"

# 对不存在的 key 进行 MSETNX
redis> MSETNX rmdbs "MySQL" nosql "MongoDB" key-value-store "redis"
(integer) 1
redis> MGET rmdbs nosql key-value-store
1) "MySQL"
2) "MongoDB"
3) "redis"

# MSET 的给定 key 当中有已存在的 key
redis> MSETNX rmdbs "Sqlite" language "python"  # rmdbs 键已经存在，操作失败
(integer) 0
redis> EXISTS language                          # 因为 MSET 是原子性操作，language 没有被设置
(integer) 0
redis> GET rmdbs                                # rmdbs 也没有被修改
"MySQL"

# 在 key 不存在时进行 SETEX
redis> SETEX cache_user_id 60 10086
OK
redis> GET cache_user_id  # 值
"10086"
redis> TTL cache_user_id  # 剩余生存时间
(integer) 49

# key 已经存在时，SETEX 覆盖旧值
redis> SET cd "timeless"
OK
redis> SETEX cd 3000 "goodbye my love"
OK
redis> GET cd
"goodbye my love"
redis> TTL cd
(integer) 2997

# PSETEX 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。
redis> PSETEX mykey 1000 "Hello"
OK
redis> PTTL mykey
(integer) 999
redis> GET mykey
"Hello"

### SETRANGE
# 对非空字符串进行 SETRANGE
redis> SET greeting "hello world"
OK
redis> SETRANGE greeting 6 "Redis"
(integer) 11
redis> GET greeting
"hello Redis"

# 对空字符串/不存在的 key 进行 SETRANGE
redis> EXISTS empty_string
(integer) 0
redis> SETRANGE empty_string 5 "Redis!"   # 对不存在的 key 使用 SETRANGE
(integer) 11
redis> GET empty_string                   # 空白处被"\x00"填充
"\x00\x00\x00\x00\x00Redis!"

### GETRANGE
redis> SET greeting "hello, my friend"
OK
redis> GETRANGE greeting 0 4          # 返回索引0-4的字符，包括4。
"hello"
redis> GETRANGE greeting -1 -5        # 不支持回绕操作
""
redis> GETRANGE greeting -3 -1        # 负数索引
"end"
redis> GETRANGE greeting 0 -1         # 从第一个到最后一个
"hello, my friend"
redis> GETRANGE greeting 0 1008611    # 值域范围不超过实际字符串，超过部分自动被符略
"hello, my friend"
```

#### incr/incrby/incrbyfloat/decr/decrby

```bash
### incr (计数器)
redis> SET page_view 20
OK
redis> INCR page_view
(integer) 21
redis> GET page_view    # 数字值在 Redis 中以字符串的形式保存
"21"

### INCRBY
# key 存在且是数字值
redis> SET rank 50
OK
redis> INCRBY rank 20
(integer) 70
redis> GET rank
"70"

# key 不存在时
redis> EXISTS counter
(integer) 0
redis> INCRBY counter 30
(integer) 30
redis> GET counter
"30"

# key 不是数字值时
redis> SET book "long long ago..."
OK
redis> INCRBY book 200
(error) ERR value is not an integer or out of range

### INCRBYFLOAT
# 值和增量都不是指数符号
redis> SET mykey 10.50
OK
redis> INCRBYFLOAT mykey 0.1
"10.6"

# 值和增量都是指数符号
redis> SET mykey 314e-2
OK
redis> GET mykey                # 用 SET 设置的值可以是指数符号
"314e-2"
redis> INCRBYFLOAT mykey 0      # 但执行 INCRBYFLOAT 之后格式会被改成非指数符号
"3.14"

# 可以对整数类型执行
redis> SET mykey 3
OK
redis> INCRBYFLOAT mykey 1.1
"4.1"

# 后跟的 0 会被移除
redis> SET mykey 3.0
OK
redis> GET mykey                                    # SET 设置的值小数部分可以是 0
"3.0"
redis> INCRBYFLOAT mykey 1.000000000000000000000    # 但 INCRBYFLOAT 会将无用的 0 忽略掉，有需要的话，将浮点变为整数
"4"
redis> GET mykey
"4"

### DECR
# 对存在的数字值 key 进行 DECR
redis> SET failure_times 10
OK
redis> DECR failure_times
(integer) 9

# 对不存在的 key 值进行 DECR
redis> EXISTS count
(integer) 0
redis> DECR count
(integer) -1

# 对存在但不是数值的 key 进行 DECR
redis> SET company YOUR_CODE_SUCKS.LLC
OK
redis> DECR company
(error) ERR value is not an integer or out of range

### DECRBY
# 对已存在的 key 进行 DECRBY
redis> SET count 100
OK
redis> DECRBY count 20
(integer) 80

# 对不存在的 key 进行DECRBY
redis> EXISTS pages
(integer) 0
redis> DECRBY pages 10
(integer) -10

```

限速器

```lua
FUNCTION LIMIT_API_CALL(ip)
current = LLEN(ip)
IF current > 10 THEN
    ERROR "too many requests per second"
ELSE
    IF EXISTS(ip) == FALSE
        MULTI
            RPUSH(ip,ip)
            EXPIRE(ip,1)
        EXEC
    ELSE
        RPUSHX(ip,ip)
    END
    PERFORM_API_CALL()
END
```

#### setbit/getbit/bitcount/bitop

```bash
###setbit 语法： SETBIT key offset value
redis> SETBIT bit 10086 1
(integer) 0
redis> GETBIT bit 10086
(integer) 1
redis> GETBIT bit 100   # bit 默认被初始化为 0
(integer) 0

### bitcount
redis> BITCOUNT bits
(integer) 0
redis> SETBIT bits 0 1          # 0001
(integer) 0
redis> BITCOUNT bits
(integer) 1
redis> SETBIT bits 3 1          # 1001
(integer) 0
redis> BITCOUNT bits
(integer) 2

### bitop
# 语法： BITOP operation destkey key [key ...]
# - BITOP AND destkey key [key ...] ，对一个或多个 key 求逻辑并，并将结果保存到 destkey 。
# - BITOP OR destkey key [key ...] ，对一个或多个 key 求逻辑或，并将结果保存到 destkey 。
# - BITOP XOR destkey key [key ...] ，对一个或多个 key 求逻辑异或，并将结果保存到 destkey 。
# - BITOP NOT destkey key ，对给定 key 求逻辑非，并将结果保存到 destkey 。
redis> SETBIT bits-1 0 1        # bits-1 = 1001
(integer) 0
redis> SETBIT bits-1 3 1
(integer) 0
redis> SETBIT bits-2 0 1        # bits-2 = 1011
(integer) 0
redis> SETBIT bits-2 1 1
(integer) 0
redis> SETBIT bits-2 3 1
(integer) 0
redis> BITOP AND and-result bits-1 bits-2
(integer) 1
redis> GETBIT and-result 0      # and-result = 1001
(integer) 1
redis> GETBIT and-result 1
(integer) 0
redis> GETBIT and-result 2
(integer) 0
redis> GETBIT and-result 3
(integer) 1
```



### List

redis的list实际上是一个双向链表

- 在两边插入效率会比较高
- 在中间插入的复杂度是O(n)

#### lpush/lpop/rpush/rpop

**把list用做栈或者队列**

```bash
## lpush
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

## rpush
127.0.0.1:6379[2]> rpush list four   # 右侧插入
(integer) 4
127.0.0.1:6379[2]> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "four"

## lpop
127.0.0.1:6379[2]> lpop list # 左侧pop
"three"

## rpop
127.0.0.1:6379[2]> rpop list # 右侧pop
"four"
127.0.0.1:6379[2]> lrange list 0 -1
1) "two"
2) "one"


```



#### llen/lset/lindex

```bash
## llen
127.0.0.1:6379[2]> llen list
(integer) 2

## lset
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
redis> LLEN list  # 列表长度为 1
(integer) 1
redis> LSET list 3 'out of range'
(error) ERR index out of range

## lindex
127.0.0.1:6379[2]> lindex list 1
"one"
```

#### lrem/trim

```bash
## lrem
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

## trim
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

#### sadd
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

Redis的Hash可以看做是一个带值的集合，其键值（key）不能重复。

#### hset

```bash
redis> HSET website google "www.g.cn"       # 如果 field 是哈希表中的一个新建域，并且值设置成功，返回 1 。
(integer) 1
redis> HSET website google "www.google.com" # 如果哈希表中域 field 已经存在且旧值已被新值覆盖，返回 0 。
(integer) 0
```

#### hsetnx

```bash
redis> HSETNX nosql key-value-store redis
(integer) 1
redis> HSETNX nosql key-value-store redis       # 操作无效，域 key-value-store 已存在
(integer) 0
```



#### hget

```bash
# 域存在
redis> HSET site redis redis.com
(integer) 1
redis> HGET site redis
"redis.com"
# 域不存在
redis> HGET site mysql
(nil)
```

#### hexists

```bash
redis> HEXISTS phone myphone # 如果哈希表不含有给定域，或 key 不存在，返回 0 。
(integer) 0
redis> HSET phone myphone nokia-1110   # 如果哈希表含有给定域，返回 1 。
(integer) 1
redis> HEXISTS phone myphone
(integer) 1
```

#### hlen

```bash
redis> HSET db redis redis.com
(integer) 1
redis> HSET db mysql mysql.com
(integer) 1
redis> HLEN db
(integer) 2
redis> HSET db mongodb mongodb.org
(integer) 1
redis> HLEN db # 哈希表中域的数量。
(integer) 3
```

#### hkeys

```bash
# 哈希表非空
redis> HMSET website google www.google.com yahoo www.yahoo.com
OK
redis> HKEYS website # 返回一个包含哈希表中所有域的表。
1) "google"
2) "yahoo"

# 空哈希表/key不存在
redis> EXISTS fake_key
(integer) 0
redis> HKEYS fake_key  # 当 key 不存在时，返回一个空表。
(empty list or set)
```

#### hvals

```bash
# 非空哈希表
redis> HMSET website google www.google.com yahoo www.yahoo.com
OK
redis> HVALS website
1) "www.google.com"
2) "www.yahoo.com"

# 空哈希表/不存在的key
redis> EXISTS not_exists
(integer) 0
redis> HVALS not_exists
(empty list or set)
```



#### hmset

```
redis> HMSET website google www.google.com yahoo www.yahoo.com
OK
redis> HGET website google
"www.google.com"
redis> HGET website yahoo
"www.yahoo.com"
```

#### hmget

```bash
redis> HMSET pet dog "doudou" cat "nounou"    # 一次设置多个域
OK
redis> HMGET pet dog cat fake_pet             # 返回值的顺序和传入参数的顺序一样
1) "doudou"
2) "nounou"
3) (nil)                                      # 不存在的域返回nil值
```

#### hgetall

```bash
redis> HSET people jack "Jack Sparrow"
(integer) 1
redis> HSET people gump "Forrest Gump"
(integer) 1
redis> HGETALL people
1) "jack"          # 域
2) "Jack Sparrow"  # 值
3) "gump"
4) "Forrest Gump"
```

#### hincrby

#### hdel

```bash
# 测试数据
redis> HGETALL abbr
1) "a"
2) "apple"
3) "b"
4) "banana"
5) "c"
6) "cat"
7) "d"
8) "dog"

# 删除单个域
redis> HDEL abbr a
(integer) 1

# 删除不存在的域
redis> HDEL abbr not-exists-field
(integer) 0

# 删除多个域
redis> HDEL abbr b c
(integer) 2

redis> HGETALL abbr
1) "d"
2) "dog"
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

