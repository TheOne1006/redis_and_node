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






- - -
