# Redis

## 简介

### NoSQL 数据库

- 不是关系型数据库
- 不支持 SQL 语法
- 存储结构和传统的关系数据库存储完全不一样，NoSQL 主要是以 key:value 的形式存在的
- 不同的 NoSQL 数据库都有不同的 api ，以及各自的适用场景，比较常见的是：Redis 和 Mongodb

SQL 和 NoSQL 数据库的区别

- 适用场景不一样，SQL 适合使用关系复杂的数据场景，比如：支持事务等处理，NoSQL 主要用在关系不复杂的情况；
- NoSQL 的查询速度会比 SQL 快速

### Redis 简介

[官方网址](https://redis.io/)
[中文官方网址](http://www.redis.cn/)

- C 语言表现捏、支持网络，数据存储在内存里同时支持持久化的日志型、ket-value 数据库，知多基本上所有的编程语言
- 查询速度快速，主要用在对性能要求较快的存储、查询等情景，比如缓存和消息队列等

- 支持：It supports data structures such as strings, hashes, lists, sets

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

### Redis 特性

- 支持数据持久化，可以将内存的数据保存在磁盘里，重启的时候可以再次加载到内存中
- 不仅支持简单的 key-value 类型数据，还提供了 list、set、zset、hash 等数据结构
- 支持数据备份，master-slave 模式的数据备份
- 性能非常高，读写都很快
- 支持原子性，也就是多线程多进程的良好支持
- 支持 publish、subscribe、通知、key 过期等特性，
- 应用广泛，基于特性使用场景非常广泛，登录请求、验证码过期、购物车、缓存、session 共享等等

## 安装和配置

安装后的一些文件：

- redis-server：redis 服务器
- redis-cli：命令行客户端
- redis-benchmark：性能测试工具
- redis-check-aof：AOF文件修复工具
- redis-check-rdb：RDB 文件检索工具

## 数据类型

存储形式：key: value

key：string
value：string、hash、list、set（集合，元素不能重复）、zset（有序集合，按照有序排列）

### 通用 api

更多的命令可以参考：[Redis 键(key) 命令](https://www.redis.net.cn/order/)

正则匹配：keys pattern --> keys *
判断是否存在：exists key [key ...] -->返回 0 或者 1
查看 value 类型：type key --> 返回对应的类型
删除 key：del key [key ...] --> 返回 0 或者 1
设置已经存在的 key过期时间：expire key seconds --> 返回 0 或者 1
查看有效时间：ttl key --> 返回数字，还剩的秒数，过期的话会返回 -2

### string

- 选择数据库：select index
- 设置 value：set key value [EX seconds|PX milliseconds|KEEPTTL] [NX|XX]
- 设置过期时间：setex key seconds value
- 设置多个 value：mset key value [key value ...]
- 追加数据：append key value
- 获取对应值：get key
- 获取多个值： mget key [key ...]

### hash

- 设置单个/多个属性：hset key field value [field value ...] --> 返回 int 几个区域返回几个
- 获取属性：hkeys key --> 返回当前的所有键值对
- 获取多个属性的值：hmget key field [field ...] --> hmget sunyunxian name city
- 获取所有属性的值：hvals ok --> 返回所有的值
- 删除 hash 某个属性： hdel key field [field ...]--> 返回 int，0或者1

### list

从左侧插入数据：lpush key element [element ...]
从右侧插入数据：rpush key element [element ...]

查看当前 list 数据：lrange key start stop
> 和 python 规则一样，比如所有的可以用 lrange person 0 -1

指定位置插入数据：linsert key BEFORE|AFTER pivot element
设置指定位置的数据：lset key index element

删除元素：lrem key count element
> count > 0：从头移除，count < 0：从尾往头移除，count = 0：删除所有

### set

set 是没有修改操作的

添加元素：sadd key member [member ...]

查询元素：smembers key --> 返回所有的元素
删除元素：srem key member [member ...]

### zset

和 set 不同的是这个集合唯一且有序的，而且会关联一个 double 类型的 score，表示权重，权重是排序的一句，会根据这个权重进行排序

添加成员：zadd key [NX|XX] [CH] [INCR] score member [score member ...] --? 返回元素的个数 int

查询成员：zrange key start stop [WITHSCORES]
查询权值在某个全家的成员：zrangebyscore key min max [WITHSCORES] [LIMIT offset count]
查询某个成员的 score 值：zscore key member
删除成员：zrem key member [member ...]

## Python 操作Redis

[Pypi Redis modele 更多介绍](https://pypi.org/project/redis/)

```python
>>> import redis
>>> r = redis.Redis(host='localhost', port=6379, db=0, password='your password')
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

Redis：存储 session 信息

## Redis 搭建主从服务

可以搭建在一台电脑，也可以是两台电脑/多台电脑，主要能保持通讯就可以了；

要在配置文件中绑定一个参数：`slaveof` 进行绑定

主从的好处很多，master 和 多个 slave 分别负责不同的功能，比较常用的是读写分离和数据备份功能，可以提升性能；

即使不使用集群，生产环境也必须使用主从服务，因为一旦挂了一个，那业务就是毁灭性灾难了；

## Redis 集群

主从的配置有一个缺点，就是 master 只有一个，如果 master 挂了，那业务也就完了，所以集群的概念也就来了，集群还会在不同的地区设置，不同的地区请求不同的集群，提升性能，同时每个主节点还是会有一个/多个 slave 的，用作读写分离和数据备份。如果集群的节点少于三个，集群就不能创建成功，同时如果活节点少于一个值，那么这个集群也会挂掉；

Redis 集群：简单理解就是有多个计算机，同时每个计算机又开启了多个服务，其实也就是 master - node 的关系，必然会存在一个 master 这个 master 主要做调度，而不是处理业务；

这个坑很多，集群很大的时候管理是个问题，现在也有很多方案进行管理，一般都是运维或者技术部门进行的 Redis 集群方案，目的主要是防止数据丢失、减少压力、动态扩容、负载均衡等等；

集群里面有个槽（slots）的概念，集群中设置数据；

## Python和Redis集群交互

就是将配置传递到 python 中，进行一系列的配置，然后处理业务就可以了；


## Redis 哨兵模式
