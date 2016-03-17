## string类型操作

### getset key newValue

```bash
redis 127.0.0.1:6379> set str1 old
OK
redis 127.0.0.1:6379> get str1
"old"
redis 127.0.0.1:6379> getset str1 new
"old"
redis 127.0.0.1:6379> get str1
"new"
```


### mget key ....

```bash
redis 127.0.0.1:6379> mget str1 a ccc
1) "newttt"
2) "2"
3) (nil)
```
