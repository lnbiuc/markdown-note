# 简介

## 什么是Redis

Redis是内存高速缓存数据库。全称为：Remote Dictionary Server（远程数据服务）。

Redis由c语言编写，key-value存储系统，支持string、list、set、zset、hash物种数据类型。

Redis可用于缓存、事件发布或订阅、高速队列等场景。基于内存，可持久化。

## Redis的优势

- 读写性能优异
- 丰富的数据类型
- 原子性
- 丰富的特性（发布订阅、通知、过期）
- 支持持久化（RDB和AOF方式持久化）
- 分布式

## Redis可以做什么

- 数据缓存
- 限时业务
- 计数器（incrby）
- 分布式锁（基于Redisson实现分布式锁）
- 限流（使用Redis中的List作为消息队列）
- 消息队列

# 安装/启动

## 安装

[Centos8 安装Redi:link:](https://www.beyondhorizon.top/article/OgSVnkzF)

## 启动

```bash
# 在前台启动
redis-server
# 在后台启动
brew services start redis
# 关闭
brew services stop redis
```

## 连接

```bash
redis-cli
```

>默认6379端口
>
>redis默认有16个数据库，0-15
>
>使用`select 1`来切换

# 基础命令

- `keys *`查看当前数据库中有哪些key（有16个数据库）
- `exists key`判断这个key是否存在
- `type key`查看这个key的类型
- `del key`或`unlink key`删除某个key
    - `unlink`异步删除
- `expire key 10`设置key过期时间 秒
- `ttl key`查看key多少秒过期 -1永不过期 -2已经过期
- `select`切换数据库
- `dbsize`查看当前数据库中key数量
- `flushdb`清除当前数据库所有key

# 基础类型（5种）

## string（字符串）

> - string类型是二进制安全的，可以包含任何数据
> - string是redis的基本数据类型，一个value最大512Mb

### 命令

- `set <key> <value>`
- `get <key>`查看
- `append <key> <value>`在key后追加value
    - 如果这个key不存在，则会创建
- `strlen <key>`获取这个key对应value长度
- `setnx <key> <value>`只有当key不存在是创建
- `incr <key>`这个key的value+1
- `decr <key>`value-1
- `incrby/decrby <key> <num>`key的value加num或者减num

这些操作是原子性操作，在执行操作期间不会别别的线程打断，因为redis是单线程

- `mset <key> <value> <key> <value> <key> <value>`一次设置多个ky
- `mget <key> <key> <key>`一次获取多个key
- `msetnx <key> <value> <key> <value>`一次设置多个 当key不存在时候
    - 原子性操作 一个失败全部失败
- `getrange <key> <开始> <结束>`获取key中第<开始><结束>中间的所有value
- `setrange <key> <offset> <value>`在key中第offset之后覆盖设置value
- `setex <key> <过期时间> <value>`在设置key时设置过期时间
- `get set <key> <value>`取之前的，取完之后再设置新的

### 数据结构

Arraylist

### 使用场景

1. 缓存：使用Redis作为MySQL缓存，降低MySQL读写压力
2. 计数器：Redis是单线程的优势
3. session：spring session + redis实现session共享

## list（列表）

> 单键多值，底层双向链表

### 命令

- `lpush/rpush <key> <v1> <v2> <v3>`从双向链表左右两边push值
- `Large <key> 0 -1`编列链表，获取所有
- `rpop/lpop <key>`从左右获取并删除值
- `rpoplpush <k1> <k2>`从k1右边pop一个值，在push到k2左边
- `lindex <key> idnex`获取第index个值
- `llen <k>`获取长度
- `linsert before/after <v1> <v2>`在v1前/后插入v2
- `lrem <key> <n> <v>`从做百年删除n个value
- `lset <k> <idnex> <v>`将index设置成v

### 数据结构

元素较少的情况下是ziplist，元素多了之后会将多个ziplist组成一个quicklist

### 使用场景

1. 消息队列

## set（集合）

> set集合中的元素不能重复，底层hash表，增删改查都是O(1)复杂度

### 命令

- `sadd <k> <v1> <v2>`添加
- `smamber <k>`取出该集合中所有值
- `sismamber <k> <v>`判断是否存在这个value
- `scard <k>`返回这个集合长度
- `srem <k> <v> <v>`删除
- `spop <k>`随机取出并删除一个
- `srandmameber <k> <n>`随机取出n个，不删除
- `smove <k1> <k2> <v>`把k1中的value取出来，放到k2中
- `sinter <k1> <k2>`返回 交集
- `sunion <k1> <k2>` 返回 并集
- `sdiff <k1> <k2>`返回差集

### 使用场景

1. 点赞、转发、收藏等

## hash（散列）

> 类似于java中的map<string,object>，key是对象名，value里边是fild和value

### 命令

- `hset <k> <field> <value>`设置key 和key中的field和value
- `hget <k> <field>` 获取k中field中的value
- `hmset <k> <field1> <v1> <field2> <v2>`一次设置多个
- `hexists <k1> <field>`查看key中的filed是否存在
- `hkeys <key>`获取该key的所有field
- `havals <key>`获取该key的所有vlaue，没有field
- `hincrby <k> <f> <n>`给f的value加n
- `hsetnx <k> <f> <v>`filed不存在则设置value

### 数据结构

当field-value长度短且个数少时使用ziplist，否则使用hashtable

### 使用场景

1. 缓存

## Zset（有序集合）

> 有序集合
>
> 每个元素都有评分（score），使用这个score来给集合排序，score可以重复
>
> 加权排序set，类似于Java中优先队列，只不过redis中将队列更换为set

### 命令

- `zadd <k> <score> <k> <score> <k> <score>`添加key和score，会根据score排名
- `arange <k> <start> <stop> withscores`获取scores在开始和结束之间的元素
- `zrangebyscore <k> <mim> <max>`返回并且排名 最小scores和最大scores
- `zincrbg <k> <n> <v>`为元素v添加n的scores
- `zrem <k> <v>`删除
- `zcount <k> <min> <max>`统计min scores 和max scores之间元素个数
- `zrank <k> <v>`获取该元素的排名，从0开始

### 数据结构

hash 跳跃表

### 使用场景

1. 排行榜

# 特殊类型（3种）

> 以下内容的学习均来自于此，使用了代码片段
>
> [原文地址:link:](https://www.pdai.tech/md/db/nosql-redis/db-redis-data-type-special.html#hyperloglogs%E5%9F%BA%E6%95%B0%E7%BB%9F%E8%AE%A1)

## HyperLogLogs（基数统计）

### 什么是基数

两个集合的全集

例如：A = {1,2,3,4,5,6} ; B = {5,6,7,8,9} ; 基数 = {1,2,3,4,7,8,9} ，即不重复元素。

### 使用场景

统计和计数，用于统计注册IP数，每日访问IP书，页面实时UV，在线用户数等。

### 优势

基于基数估算的算法，能比较准确的估算出基数，可以使用少量固定的内存存储并识别集合中唯一的原色。

这个算法并不一定准确，有0.81%的错误率，基数统计适合接受一定容错的场景，比如统计访客数量等。

### 命令

```sh
127.0.0.1:6379> pfadd key1 a b c d e f g h i	# 创建第一组元素
(integer) 1
127.0.0.1:6379> pfcount key1					# 统计元素的基数数量
(integer) 9
127.0.0.1:6379> pfadd key2 c j k l m e g a		# 创建第二组元素
(integer) 1
127.0.0.1:6379> pfcount key2
(integer) 8
127.0.0.1:6379> pfmerge key3 key1 key2			# 合并两组：key1 key2 -> key3 并集
OK
127.0.0.1:6379> pfcount key3
(integer) 13
```

## Bitmap（位存储）

Bitmap是通过二进制记录信息，只有 0 和 1 两个状态

### 使用场景

1. 统计用户登陆信息，登陆和未登陆
2. 统计用户点赞信息，点了和没点
3. 统计员工打卡信息，打了和没打

### 命令

- 使用`setbit <key> <offset> <value>`写入数据，其中key可以任意，offset只能为Integer，value只能为0或1

    如果要统计员工一周的打卡情况，key可以设置为sign，offset设置为0-6，打卡为0，未打卡为1

```sh
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 1
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 0
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 1
(integer) 0
```

- 使用`getbit <key> <offset>`来查看某一个key的状态，返回0或1

```sh
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 5
(integer) 0
```

- 使用`bitcount <key>`统计某一项数据的总数

```sh
127.0.0.1:6379> bitcount sign
(integer) 3
```

## geospatial（地理位置）

可以用户推算地理位置信息，如两地之间距离等

### 命令

- `geoadd`添加位置

```sh
127.0.0.1:6379> geoadd china:city 118.76 32.04 nanjing 112.55 37.86 taiyuan 123.43 41.80 shenyang
(integer) 3
127.0.0.1:6379> geoadd china:city 144.05 22.52 shengzhen 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 3
```

- `geopop`获取指定成员的地理位置

```sh
127.0.0.1:6379> geopos china:city taiyuan nanjing
1) 1) "112.54999905824661255"
   1) "37.86000073876942196"
2) 1) "118.75999957323074341"
   1) "32.03999960287850968"
```

- `geodist`获取两地之间的距离，可以指定单位

```sh
127.0.0.1:6379> geodist china:city taiyuan shenyang m
"1026439.1070"
127.0.0.1:6379> geodist china:city taiyuan shenyang km
"1026.4391"
```

- `georadius`

> [这里有详细解释:link:](https://cloud.tencent.com/developer/section/1374022)

附近的人，获得所有附近的人的地址, 定位, 通过半径来查询

- `georadiusbymember`

[link:link:](https://cloud.tencent.com/developer/section/1374023)

显示与指定成员一定半径范围内的其他成员
