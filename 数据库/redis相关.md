# redis相关

## 0.

```
1应用场景：高并发，磁盘I/o速度慢

2非关系型数据库

（关系型数据库与非关系型数据库最大的区别就是，关系型数据库支持表结构）

3redis不支持表结构，redis是键值对结构



4redis持久化就是指：把数据从内存复制到磁盘中去

5redis常作为缓存中间件，客户端---->redis------>mysql（硬盘）

6redis支持持久化rdb，aof

7redis支持数据备份，主从复制

8redis默认支持16个数据库

9redis 具有原子性，单个基本操作是原子的，但是redis事务不具有原子性	
10redis支持发布订阅(观察者模式)

```



## 1.redis常用命令

```
0.redis启动：	redis-cli

1.切换数据库 	select number

2.查看当前数据库大小 	dbsize

3.查看当前数据库的key值 		keys */keys 	k?（问号表示匹配一个符号）

4.删除key值 					del key

5.清空当前数据库			flushdb

​	清空所有数据库			flushall

6.移动数据库				move key number（0-15）

7判断key是否存在		exsits key

8查看key数据类型		type k1

9设置key过期时间，查看key时间

​	expire key number

​	ttl key（-1表明永不过期，-2表示已经过期）
```



## 2.redis五大常用数据类型

### 1.string

```
1.设置value		set key value

​	2.获取value		get key 

​	3.大量设置		mset key1 value1 key2 value2.。。。

​	4.大量获取		mget key1 key2 key3.。。。。

​	5.获取字串	getrange key start end

特殊的，获取全部	

getrange key 0 -1

​	6.设置字串	setrange	key offset value

​	7.给key设置过期时间与新值

​	setex key time newvalue(setex=set expire)

​	8key++(只能int类型)		incr key

​	9key+number				incrby	key number
```

### 2.hash

```
数据结构：key-value(但是value又是一个key-value)

1.插入1个 			hset hash key value

2.获取1个			 hget hash key

3.插入多个			hmset  hash  key1 value1 key2 value2 key3 value3

4.获取多个			hmget  hash  key1 key2 key3 key4

5.获取keys 			hkeys   hash

6.获取values		 hvals	hash
```

### 3.list

```
	1.lpush list	number1 number2 number3

​	2.rpush list	number1 number2 number3

​	3.遍历list		lrange list 0 -1

​	4.lpop	 list

​	5.rpop 	list

​	6.设置或更改value(支持下标操作)		lset	list	index	value

​	7.获取下标元素										                  lindex list index

​	8.删除m个n																lrem list m n

​	9.只留下m到n，剩下的不要了			  					ltrim	list m n

​	10.在某个元素前后插入数据				linsert list before/after	pivot value


```

### 4.set

```
set无序容器，元素唯一，元素无顺序，底层使用的是哈希

​	1.插入	sadd key value1 value2 value3

​	2.查看所有成员	smembers set

​	3.查看有几个元素	scards set

	4. 判断是否为set成员		sismember	set  value
	5. 将元素从set1移动到set2      smove  set1 set2  value
	6. 删除元素       srem set value
	7.  在集合中随机选出num个数（不删除）     srandmember  set   number
	8. 在集合中随机删除三个元素        spop   set  number

   9.取差集       sdiff   set1 set2

​    取交集        sinter   set1 set2

​    取并集       sunion    set1 set2


```

### 5.sorted set

```
成员不允许重复，权重值允许重复

1.添加     zadd  set  权重1 value1 权重2 value2 。。。。。

2.查看成员数目		zcard 	set

3.统计m到n之间有几个数      zcount set m n

4.查找m下标与n下标范围之间的元素    zrange set m n  (withscores)

5.查看权重	    zscore set value  

6.反向排序(按照权重)，从m到n     zrevrangebyscore set m n

7.保证权重值一致的前提条件下，zrangebylex

特例：zrangebylex set  -  +


```

## 3.redis相关配置文件

```
1.redis,down机，	$shutdown///在外面 sudo  kill -9

2.查看进程					ps -elf|grep redis

3.不在redis目录下启动redis server(非守护进程)												sudo     redis-server

​	启动指定conf文件(带配置文件)(以守护进程的方式起来)							sudo redis-server 7379.conf
```



## 4.redis持久化(这一部分写的不全，可参考lili老师笔记)

```
1.将redis中的数据从内存保存的磁盘，分两种 RDB / AOF

2.RDB：将数据定期保存到磁盘（dump到）（默认）

 RDB运行原理，在指定目录下生成一个dump.rdb文件，redis每次重启会通过加载dump.rdb文件恢复数据.

dump.rdb文件默认在 var/lib/redis/6379

注：你可以手动执行save命令保存

触发快照方式（3）

1.执行shutdown

2.flushall

3.手动save，dump.rdb会被立即更新









 

3. AOF：每次将写命令保存到磁盘中去(主流)(略)


```

