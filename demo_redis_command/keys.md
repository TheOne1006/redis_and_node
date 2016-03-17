## key 案例展示

### KEYS pattern
```bash
#首先创建一些 key，并赋上对应值
redis 127.0.0.1:6379> SET w3c1 redis
OK
redis 127.0.0.1:6379> SET w3c2 mysql
OK
redis 127.0.0.1:6379> SET w3c3 mongodb
OK
redis 127.0.0.1:6379> KEYS w3c*
1) "w3c1"
2) "w3c2"
3) "w3c3"

#获取 redis 中所有的 key 可用使用 *
KEYS *
```
