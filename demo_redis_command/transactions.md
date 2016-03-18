## Redis 实物


```bash
# 开启
redis 127.0.0.1:6379> MULTI
OK
# 命令入队
redis 127.0.0.1:6379> SET book-name "nodejs rumen"
QUEUED
redis 127.0.0.1:6379> get book-name
QUEUED
redis 127.0.0.1:6379> sadd tag "c++"
QUEUED
# 执行
redis 127.0.0.1:6379> exec
1) OK
2) "nodejs rumen"
3) (integer) 1

```
