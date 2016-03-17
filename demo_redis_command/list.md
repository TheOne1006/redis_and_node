## Redis 列表  


### LINDEX key index


```bash
redis 127.0.0.1:6379> LPUSH mylist hello
(integer) 1
redis 127.0.0.1:6379> lpush mylist world
(integer) 2
redis 127.0.0.1:6379> lindex mylist 0
"world"
redis 127.0.0.1:6379> lindex mylist 1
"hello"
```
