# Node.js And Redis 实践

## Redis 简介

`Redis`是基于 BSD 开源的使用 ANSI C 语言编写,支持网络,可基于 _内存_ 亦可 _持久化_ 的 _日志型_ ,  _key-value_ 数据库.


强调:  
1. key-value 数据库
2. 内存数据库, 也可以将数据保存到磁盘中.
3. 五中数据类型:
  1. string: 最基础, 任何形式的字符串,包括二进制数据
  2. hash(哈希类型):
  3. list(链表类型)
  4. set(无序集合类型)
  5. zset(有序集合类型)


## 安装

```bash
sudo apt-get install redis-server
```

默认端口: `6379`

## shell

使用 redis-cli 链接

```bash
redis-cli
> PING
PONG
# 验证链接正常
```


## 基本使用 之 SHELL

__注意__:

1. 命令不区分大小写, 建议大写
2. 数字值以字符串保存


### 字符串类型操作

- SET key value: 设置 key 的值为 value, 成功返回 OK
- GET key: 获取 key 值, 成功返回 key值
- INCR key: 值自加1
  - 如果key 不存在, 初始化为 0
  - 如过 值不为整数, 会提示错误

### 哈希类型操作

- HSET key field value [field value ..... ]: 设置key 的 field 字段值为 value
  - success: return 1
  - 如果 field 已存在 返回 0
- HGET key field : 获取 key的 field 字段值
- HGETALL key : 获取key 所有字段和其值

demo:  
```bash
redis 127.0.0.1:6379> HSET author name theone
(integer) 1
redis 127.0.0.1:6379> HSET author sex man
(integer) 1
redis 127.0.0.1:6379> HSET author sex man
(integer) 0
redis 127.0.0.1:6379> HGET author name
"theone"
redis 127.0.0.1:6379> HGETALL author
1) "name"
2) "theone"
3) "sex"
4) "man"
```

### 链表类型操作

- LPUSH key value [ value .... ]
  - left push, 左链中添加
  - 返回链表长度
  - 一次只能传一个值 __?__
- RPUSH key value [ ...]
  - 参考上文
- LPOP key: 左侧第一个元素
  - left pop
- RPOP key:
  - 参考上文
- LRANGE key start stop:
  - 获取链表片段
  - 不会改变原链表的值
  - 支持负值索引

Q:
一次 push 1个 元素 ?


### 无序集合类型

- SADD key member [member ... ]
  - SET ADD
  - 添加一个或多个元素
  - 唯一不重复
  - 类似 ES6 中的 Set
  - 返回元素数量
  - 为什么一次只能添加一个
- SREM key member [member ...]
  - SET REMOVE
  - 删除一个或多个元素
  - 返回元素数量
- SMEMBERS key: 返回集合所有元素
  - SET MEMBERS
- SINTER key1 key2 ...: 交集
  - SET intersection
- SDIFF key1 key2 ...: 差集
  - SET DIFF
- SUNION key1 key2 ...: 并集
  - SET UNION


### 有序集合类型

Z :

- ZADD key score member [...]
  - ZSET ADD
  - socre: 下标/分数
  - member 元素值
  - 添加
- ZREM key member
  - ZSET remove
- ZRANGE key start stop [WITHSCORES]: 按分数从小到大排序
  - ZSET RANGE key start stop
  - WITHSCORES 如果分数相同, 按照元素字典顺序排列
- ZREVRANGE key start stop [WITHSCORES] : 按分数从大到小排序
  - ZSET REV RANGE key start stop





## Redis 键命令 `COMMAND KEY_NAME`

[参考地址](http://www.runoob.com/redis)

1. DEL key: 删除key
  - success: 1
2. DUMP key:
  - 序列化key: 返回序列化的值
  - v >= 2.6.0
3. EXISTS key: 是否存在
  - 存在返回 1
  - 否则返回 0
4. EXPIRE key time_in_seconds
  - 设置过期时间,单位 秒, 几秒之后过期
  - 类似命令:
  - PEXPIRE key milliseconds: 毫秒记过期时间
  - PEXPIREAT key milliseconds-timestamp : 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计
5. KEYS pattern: 查询符合 pattern 模式的 key, 返回 key 的列表
  - demo_redis_commond/keys
6. MOVE key database : 将key 移动到指定数据库 db 当中
  - redis 默认使用数据库0
  - 目标数据如果存在的移动失败
7. PERSIST key: 移除key的过期时间,key将永久保存
8. PTTL key: 返回 key 剩余过期时间(毫秒)
9. TTL key: (time to live) key 剩余生存时间 (s)
  - -1 永久
10. RANDOMKEY: (random key) 当前库随机返回 一个key,
11. RENAME key newkey: 修改key 名
  - 如果 newkey 存在,会覆盖
12. RENAMENX key newkey: 仅档 newkey 不存在,可以才允许 改名
13. TYPE key : 返回 key 存储类型



## Redis 字符串

1. SET key value
2. GET key value
3. GETRANGE key start end
  - get range key
  - 字符串的字符区间
  - v >= 2.4.0
3. GETSET key newValue
  - 返回旧值,并设置新值
  - key不存在返回 nil
  - key 不是 string 类型,返回一个错误
4. MGET key [ key2 key3 ...]
  - 返回多个key值,
  - 如果某个 key 不存在/非string 返回nil
5. SETEX key timeout(s) value
  - 设置值和过期时间 (秒)
6. SETNX key value: 如果key 不存在则设置 为 value
7. SETRANGE key offset value : 复写局部字符串
  - set range key offset value
8. STRLEN key: 值的字符串长度
  - 中文: 3个长度
9. MSET key1 val1 key2 val2 ...
  - 同时设置 多个, 会覆盖原始值,key-value
10. MEETNX key1 val1 key2 val2 ...
  - 同时设置多个,不会覆盖
11. 增加 Increment , __Number__ 独有
  - INCR key : 自增 1
  - INCRBY key amount (incr by amount) : 自增以 amount
  - INCRBYFLOAT key incre : 增量为浮点
12. 减少 Decrement , __Number__ 独有
  - DECR key : 自减 1
  - DECRBY key amount:
13. APPEND key value: 将 value 追加到key值的末尾


## Redis 哈希

1. HDEL key: 删除 一个/多个 哈希表字段
2. HEXISTS key: hash表是否存在
3. HGET key field:
4. HGETALL key:
5. 增量 指定字段操作:
  - HINCRBY key field increment: 指定 field 增加 increment
  - HINCRBYFLOAT key field increment
6. HKEYS key: key 的所有字段
7. HLEN key: 字段数量
8. HMGET key field1 [field2]: 指定字段的值
9. HMSET key filed1 val1 f2 v2: hash版 的 `mset`
10. 设置  
  - HSET key filed value
  - HSETNX key filed val1
11. HVALS KEY: 表中的所有值


## Redis 列表  

1. PUSH 系列
  - LPUSH key value : 创建/插入 key的左侧列表
  - LPUSHX key value: (key列表必须存在)

2. POP 系列
  - BLPOP/BRPOP key timout : 移除 左/右 边第一个元素, 如无元素 等待 timeout(s)
  - RPOP key

3. 混合
  - BRPOPLPUSH listOut listIn timeout: listOut 右弹出,  listIn 左插入
4. 索引
  - LINDEX key index : 通过索引 获取
  - LSET key index value : 通过索引 设置元素值
5. LLEN key : 列表长度
6. 查找
  - LRANGE key start stop: 获取列表指定范围
7. 删除
  - LREM key count value: 参考count 值,移除列表与参数 value 相等的元素
  - count > 0 : 从表头开始向表尾搜索，移除与 VALUE 相等的元素，数量为 COUNT 。
  - count < 0 : 从表尾开始向表头搜索，移除与 VALUE 相等的元素，数量为 COUNT 的绝对值。
  - count = 0 : 移除表中所有与 VALUE 相等的值。

8. LTRIM key start stop: 保留 start - stop 范围内的元素



## Redis 集合

- 增
  1. SADD key member
    - 在Redis2.4版本以前， SADD 只接受单个成员值。
- 删
  1. SPOP key: 随机删除一个元素
- 改
  1. SMOVE source dest member: 将member 元素从 source 移动到 dest
- 查
  1. SCARD key : 返回集合元素数量
  2. SISMEMBER key member: member 是否存在与 key
  3. SMEMBERS key : 返回集合所有成员
  4. SRANDOMMEMBER key [count]: 随机返回一个随机元素
- 集合对比
  1. SDIFF key1 key2 ... : 差集成员列表
  2. SINTER key1 key2 ... : 交集
  3. SUNION key1 key2 ... : 并集
  4. SDIFFSTORE destination key1 key2 : key1 key2 差集 结果存在 destination 中
  5. SINTERSTORE destination key1 key2 :  
  6. SUNIONSTORE dest key1 key2 :


## Redis 事务
事务可以一次执行多个命令， 并且带有以下两个重要的保证：  
- 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。
- 事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
- 事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。

一个事务从开始到执行会经历以下三个阶段：  
1. 开始事务。
2. 命令入队。
3. 执行事务。


## Redis 连接命令

1. AUTH  password : 验证密码是否正确
2. ECHO mes : 打印字符串
3. PING : 正常 返回 PONG
4. QUIT : 关闭当前连接
5. SELECT index : 连接指定数据库, 默认 0


## Redis 服务器

1. BGREWRITEAOF : ??
2. BGSAVE : 后台异步保存当前数据库到磁盘
3. CLIENT KILL ip:port 关闭客户端连接
  - v >= 2.4.0
4. CLIENT LIST :所有连接到服务器的客户端信息和统计数据。
  - v >= 2.4
5. DBSIZE: 当前库中key数量
6. DEBUG SEGFAULT : 命令执行一个非法的内存访问从而让 Redis 崩溃，仅在开发时用于 BUG 调试。
7. FLUSHALL: 删除所有库中所有的key
8. FLUSHDB: 删除当前库中所有的 key
9. INFO : 服务器各种信息和统计数值
10. LASTSAVE: 最后一次成功保存的时间戳
11. MONITOR: 实时打印出 服务器接收到的命令
12.












- - -
