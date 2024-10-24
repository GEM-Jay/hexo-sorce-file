---
title: Redis的数据类型
tags:
  - Redis
categories:
  - 技术
topic:
  - redis
abbrlink: 1286363648
date: 2024-10-19 18:14:52
---
# Redis 数据类型

Redis 主要支持以下几种数据类型：

* **string（字符串）:** 基本的数据存储单元，可以存储字符串、整数或者浮点数。
* **hash（哈希）:**一个键值对集合，可以存储多个字段。
* **list（列表）:**一个简单的列表，可以存储一系列的字符串元素。
* **set（集合）:**一个无序集合，可以存储不重复的字符串元素。
* **zset\(sorted set：有序集合\):** 类似于集合，但是每个元素都有一个分数（score）与之关联。
* **位图（Bitmaps）：**基于字符串类型，可以对每个位进行操作。
* **超日志（HyperLogLogs）：**用于基数统计，可以估算集合中的唯一元素数量。
* **地理空间（Geospatial）：**用于存储地理位置信息。
* **发布/订阅（Pub/Sub）：**一种消息通信模式，允许客户端订阅消息通道，并接收发布到该通道的消息。
* **流（Streams）：**用于消息队列和日志存储，支持消息的持久化和时间排序。
* **模块（Modules）：**Redis 支持动态加载模块，可以扩展 Redis 的功能。

* * *

## String（字符串）

string 是 redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value。

string 类型是二进制安全的。意思是 redis 的 string 可以包含任何数据，比如jpg图片或者序列化的对象。

string 类型是 Redis 最基本的数据类型，string 类型的值最大能存储 512MB。

### 常用命令

* `SET key value`：设置键的值。
* `GET key`：获取键的值。
* `INCR key`：将键的值加 1。
* `DECR key`：将键的值减 1。
* `APPEND key value`：将值追加到键的值之后。

### 实例

redis 127.0.0.1:6379> SET runoob "菜鸟教程"
OK
redis 127.0.0.1:6379> GET runoob
"菜鸟教程"

在以上实例中我们使用了 Redis 的 **SET** 和 **GET** 命令。键为 runoob，对应的值为 **菜鸟教程**。

**注意：**一个键最大能存储 512MB。

* * *

## Hash（哈希）

Redis hash 是一个键值\(key=>value\)对集合，类似于一个小型的 NoSQL 数据库。

Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。

每个哈希最多可以存储 2\^32 \- 1 个键值对。

### 常用命令

* `HSET key field value`：设置哈希表中字段的值。
* `HGET key field`：获取哈希表中字段的值。
* `HGETALL key`：获取哈希表中所有字段和值。
* `HDEL key field`：删除哈希表中的一个或多个字段。

### 实例

DEL runoob 用于删除前面测试用过的 key，不然会报错：**\(error\) WRONGTYPE Operation against a key holding the wrong kind of value**

![](https://www.runoob.com/wp-content/uploads/2014/11/B104156B-7270-4D03-8EB3-B72D4022ED78.jpg)

redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> HMSET runoob field1 "Hello" field2 "World"
"OK"
redis 127.0.0.1:6379> HGET runoob field1
"Hello"
redis 127.0.0.1:6379> HGET runoob field2
"World"

实例中我们使用了 Redis **HMSET, HGET** 命令，**HMSET** 设置了两个 field=>value 对, HGET 获取对应 **field** 对应的 **value**。

* * *

## List（列表）

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

列表最多可以存储 2\^32 \- 1 个元素。

### 常用命令

* `LPUSH key value`：将值插入到列表头部。
* `RPUSH key value`：将值插入到列表尾部。
* `LPOP key`：移出并获取列表的第一个元素。
* `RPOP key`：移出并获取列表的最后一个元素。
* `LRANGE key start stop`：获取列表在指定范围内的元素。

### 实例

redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> lpush runoob redis
\(integer\) 1
redis 127.0.0.1:6379> lpush runoob mongodb
\(integer\) 2
redis 127.0.0.1:6379> lpush runoob rabbitmq
\(integer\) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1\) "rabbitmq"
2\) "mongodb"
3\) "redis"
redis 127.0.0.1:6379>

* * *

## Set（集合）

Redis 的 Set 是 string 类型的无序集合。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O\(1\)。

### 常用命令

* `SADD key value`：向集合添加一个或多个成员。
* `SREM key value`：移除集合中的一个或多个成员。
* `SMEMBERS key`：返回集合中的所有成员。
* `SISMEMBER key value`：判断值是否是集合的成员。

### sadd 命令

添加一个 string 元素到 key 对应的 set 集合中，成功返回 1，如果元素已经在集合中返回 0。

sadd key member

### 实例

redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> sadd runoob redis
\(integer\) 1
redis 127.0.0.1:6379> sadd runoob mongodb
\(integer\) 1
redis 127.0.0.1:6379> sadd runoob rabbitmq
\(integer\) 1
redis 127.0.0.1:6379> sadd runoob rabbitmq
\(integer\) 0
redis 127.0.0.1:6379> smembers runoob

1\) "redis"
2\) "rabbitmq"
3\) "mongodb"

**注意：**以上实例中 rabbitmq 添加了两次，但根据集合内元素的唯一性，第二次插入的元素将被忽略。

集合中最大的成员数为 232 \- 1\(4294967295, 每个集合可存储40多亿个成员\)。

* * *

## zset\(sorted set：有序集合\)

Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数\(score\)却可以重复。

### 常用命令

* `ZADD key score value`：向有序集合添加一个或多个成员，或更新已存在成员的分数。
* `ZRANGE key start stop [WITHSCORES]`：返回指定范围内的成员。
* `ZREM key value`：移除有序集合中的一个或多个成员。
* `ZSCORE key value`：返回有序集合中，成员的分数值。

### zadd 命令

添加元素到集合，元素在集合中存在则更新对应score

zadd key score member 

### 实例

redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> zadd runoob 0 redis
\(integer\) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
\(integer\) 1
redis 127.0.0.1:6379> zadd runoob 0 rabbitmq
\(integer\) 1
redis 127.0.0.1:6379> zadd runoob 0 rabbitmq
\(integer\) 0
redis 127.0.0.1:6379> ZRANGEBYSCORE runoob 0 1000
1\) "mongodb"
2\) "rabbitmq"
3\) "redis"

* * *

## 其他高级数据类似

### HyperLogLog

* 用于基数估计算法的数据结构。
* 常用于统计唯一值的近似值。

### Bitmaps

* 位数组，可以对字符串进行位操作。
* 常用于实现布隆过滤器等位操作。

### Geospatial Indexes

* 处理地理空间数据，支持地理空间索引和半径查询。

### Streams

    * 日志数据类型，支持时间序列数据。
    * 用于消息队列和实时数据处理。

<!-- 其他扩展 -->