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



- - -
