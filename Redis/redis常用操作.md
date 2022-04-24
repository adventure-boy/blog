# redis常用操作

## key相关的命令

- get(查询) ,set(插入)

set: 当存在键时覆盖

```sh
127.0.0.1:6379> set username zhagnsan
OK
127.0.0.1:6379> get username
"zhagnsan"
```

- del(删除)

```sh
127.0.0.1:6379> del username
(integer) 1
127.0.0.1:6379> get username
(nil)
127.0.0.1:6379>
```

- dump

dump:序列化键

```sh
127.0.0.1:6379> dump username
"\x00\x04lisi\x06\x00\x94o\xf5\xb2\x97^\x867"
```

- exists

exists:用于检查key是否存在

```sh]
127.0.0.1:6379> EXISTS username
(integer) 1
127.0.0.1:6379> exists password
(integer) 0
127.0.0.1:6379>
```

- ttl,pttl

ttl:应用检查一个给定key的有效时间,单位为秒

pttl:应用检查一个给定key的有效时间,单位为毫秒

```sh
127.0.0.1:6379> TTL username
(integer) -1
127.0.0.1:6379> TTL password
(integer) -2
127.0.0.1:6379>
```

-2 表示 key 不存在或者已过期；-1 表示 key 存在并且没有设置过期时间（永久有效）

- expire,pexpire

expire:用于给key设置有效时间,单位为秒

pexpire:用于给key设置有效时间,单位为毫秒

```
127.0.0.1:6379> EXPIRE username 60
(integer) 1
```

- keys

keys:查看所有的键

```sh
127.0.0.1:6379> keys *
 1) "password"
 2) "userRole::682265633886208"
 3) "permission::allList"
 4) "BUCKET_MONITOR:upload:127.0.0.1"
 5) "department::0:4363087427670016_false"
 6) "BUCKET_COUNT"
 7) "name"
```

