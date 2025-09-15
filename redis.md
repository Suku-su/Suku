# redis

#### 是什么

Remote Dictionary Service 远程字典服务
是一个开源的使用c语言编写、支持网络、可基于内存亦可持久化的日志型、Key-value数据库，并且提供了多种语言的api

![image-20250824214050623](redis.assets/image-20250824214050623.png)

#### 能干什么

1、内存存储、持久化、内存中是断电即失的、所以说持久化很重要（RDB、AOF）

2、效率高，可用于高速缓存

3、发布订阅系统

4、地图信息分析

5、计数器（浏览量，点赞等）、计时器

6、.......

#### redis的发展

```http
https://blog.csdn.net/m0_66884848/article/details/148630339?ops_request_misc=&request_id=&biz_id=102&utm_term=redis%E5%8F%91%E5%B1%95&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-148630339.142^v102^pc_search_result_base7&spm=1018.2226.3001.4187
```



## 五大数据类型

### **string**

move key 0

#### 常规操作

![image-20250908214816238](redis.assets/image-20250908214816238.png)

```bash
set key1 value1 # 设置值
get key1  #获取值
keys *  #获取所有的值
exists key1 #判断某个值是否存在
append key1 "hello"   #追加字符串,如果当前的key不存在，就相当于setkey
strlen key #获取字符串额长度！！
```



---





原子性操作

![image-20250908215328546](redis.assets/image-20250908215328546.png)

```bash
incr key # 自增1
decr ke # 自减1

```



---

获取全部、替换

![](redis.assets/image-20250908220014593.png)

```bash
 getrange key1 0 3 #截取字符串  [0,3]
 getrange key1 0 -1 #获取全部的字符串 和get key 是一样的 
 setrange key2 1  xx(-1/3) #替换指定位置出现的字符串（替换全部 /1到3位置的字符串）
```





---

#### 分布式锁

![image-20250908221543991](redis.assets/image-20250908221543991.png)

```bash
setex (set with expire ) # 设置过期时间
setnx (set with exists) #  如果不存在才可以设置返回1，如果存在就会返回0
set key 30 "value"  # 设置key的值为value，30秒后过期
setnx mykey "redis" # 如果mykey 不存在就会创建mykey，并且返回1，当有其他的人再次执行这条指令的时候就不能设置成功

```



---

#### 一次设置多个值

mset等可以保证原子性但是redis的事务 是不保证的

![image-20250908222016099](redis.assets/image-20250908222016099.png)

```bash
mset k1 v1 k2 v2 k3 v3 #同时设置多个值
mget k1 k2 k3 # 同时获取多个值
msetnx k1 v1 k2 v2 #msetnx 是一个原子性操作，要么就一起成功，要么就一起失败!
```



---

#### 设置对象，复用

![image-20250908222636665](redis.assets/image-20250908222636665.png)

```bash
# 对像
set user:1{name:zhangsan,age:3} #设置一个user:1 对象 值为 json字符保存一个对象！
#这里的key是一个巧妙的设计： user:{id}:{filed}，如此设计在redis中是完全ok的
```





### **hash**

---

map集合，key-map！时候这个值是一个集合！本质和String类型没有太大的区别，还是一个简单的key-value！

#### 常规操作

![image-20250909222056275](redis.assets/image-20250909222056275.png)

```bash
hset myhash key1 value1 # set一个具体的key-value
hget myhash key1 #获取一个字段值
hmset myhash key1 value1 key2 value2 key3 value3 # 设置多个key-value
hmget myhahsh key1 key2 key3 #获取多个字段的值
hgetall myhash # 获取全部的数据
```

#### 删除操作

![image-20250909222737172](redis.assets/image-20250909222737172.png)

```bash
hdel myhash key1 # 删除hash指定的key字段！对应value值也会对应的消失
```

#### 获取长度

![image-20250909223114018](redis.assets/image-20250909223114018.png)

```bash
hlen myhash #获取hash表的字段数量
```

#### 判断字段长度是否存在

![image-20250909223251369](redis.assets/image-20250909223251369.png)

```bash
hexists myhash key1  #判断hash中指定字段是否存在
```

#### 增减

![image-20250909225142010](redis.assets/image-20250909225142010.png)

```bash
hset myhash key1 5 #在指定增量 ！

hincrby myhash key1 1 #增1

hincrby myhash key -1 #减1

hdecrby myhash key 1# 减1

hsetnx myhash key1 hello #如果存在则不能设置，不存在才可以
```

hash变更的数据user name age，尤其是用户信息之类的，经常变动的信息！hash更适合对象存储，String更加适合字符串存储!

---



### **zset**

---

#### 常规操作

![image-20250910230521741](redis.assets/image-20250910230521741.png)

```bash
zadd myset 1 one #添加也给值

zadd myset 2 two 3 three #添加多个值
```

#### 排序如何实现

![image-20250910230810485](redis.assets/image-20250910230810485.png)

![image-20250910230936732](redis.assets/image-20250910230936732.png)

![image-20250910231924082](redis.assets/image-20250910231924082.png)

```bash
zrangebyscore salary -inf +inf #显示全部的用户  从大到小

zrevrange salary 0 -1 #从大到小进行排序

zrangebyscore salary -inf +inf withscores #显示全部的用户并且附带成绩

zrangebyscore salarry -inf 2500 withscores #显示工资小于2500的员工的升序排序 

```

#### 移除集合中的元素

![image-20250910231633696](redis.assets/image-20250910231633696.png)

```bash
zrem salary xiaohong #移除有序集合中的指定元素

zcard salar #获取有序集合中的个数
```

#### 获取指定区间的成员数量

![image-20250910232254693](redis.assets/image-20250910232254693.png)

```bash
zcount myset 1 2   # 获取指定区间的成员数量
```

一些api调用，

案例思路：set 排序 存储班级成绩表  ，  工资表排序

普通消息，1，重要消息，2，带权重进行判断！

排行榜应用实现，取Top N测试！











---



### **list**

#### 可以把他当成栈、队列、阻队列

所有的lis命令都是l开头的

lpush list one

lrange list 0 -1

![image-20250908225559848](redis.assets/image-20250908225559848.png)

---

#### 移除元素

![image-20250908225852094](redis.assets/image-20250908225852094.png)

#### 位置index

![image-20250908230131036](redis.assets/image-20250908230131036.png)

#### 精确移除

![image-20250908230407631](redis.assets/image-20250908230407631.png)

#### 移除两边

![image-20250908230639791](redis.assets/image-20250908230639791.png)

----

#### 移除列表然后放到其他的位置

![image-20250908230842811](redis.assets/image-20250908230842811.png)

#### 将指定下标的元素替换

![image-20250908231237300](redis.assets/image-20250908231237300.png)

#### 将某个具体的value插入到列表的某个元素前面或者是后面

![image-20250908231628228](redis.assets/image-20250908231628228.png)

---

总结：

+ 它实际是一个链表，brfore 、node after ，left ，right 都可以插入
+ 如果key 不存在，创建新的链表
+ 如果key存在，就会新增内容
+ 如果移除了所有的值，空链表，也代表不存在
+ 在两边插入或者改动值，效率最高！中间的元素，相对来说效率会低一点~

消息排队、消息队列，栈











## **set**

添加查询是否包含

![image-20250909201541253](redis.assets/image-20250909201541253.png)

```bash
sadd myset "你好" # set 集合添加元素 
sadd myset "aaaa"
sadd myset "hhhhhh"

smemebers myset          # 查看指定的set的所有的值

sismembers myset aaaa   # 判断某一个值是否在set集合中
sismembers myset world  # 判断某一个值是否在set集合中
```

#### 移除元素

![image-20250909202238443](redis.assets/image-20250909202238443.png)

```bash
scard myset #获取set集合中的元素个数

srem myset hello #移除set集合中的指定元素
```

#### 随机弹出一个数

![image-20250909202735657](redis.assets/image-20250909202735657.png)

```bash
srandmembers myset #随机弹出一个数
srandmembers myset 2# 随机弹出俩个数
```

#### 随机删除一个key/随机弹出一个key

![image-20250909203152031](redis.assets/image-20250909203152031.png)

```bash
spop myset #弹出一个随机的ky
```

#### 将一个指定的值移动到另一个指定的集合

![image-20250909211147759](redis.assets/image-20250909211147759.png)

```bash
smove myset myset2 "hello" # 将一个指定的值移动到另一个set集合
```



#### 交集、并集、差集

![image-20250909220807469](redis.assets/image-20250909220807469.png)

```bash
sdiff key1 key2 # 差集
sinter key1 key2 # 交集  共同好友就可以这样实现
sunion key1 key2 # 并集
```

使用场景：

微博，A用户将所有关注的人放在一个set集合中！将他的粉丝也放在一个集合中！

共同关注，共同爱好，二度好友，推荐好友！（六度分割理论）









---



keys * 

set name xxx

exists name     是否存在

move name 1     移除当前的key

expire name 10   (10秒过期)

append key 需要追加的内容









## 三种特殊的数据类型 

### geospatial地理位置

朋友定位，附近的人，打车的举例计算

推算地理位置的信息，两地之间的距离，方圆几里的人

城市经纬度查询

```bash
http://jingweidu.757dy.com/
```

> 只有六个命令

![image-20250911215851551](./redis.assets/image-20250911215851551.png)

#### getadd 

添加地址位置

![image-20250911221309627](./redis.assets/image-20250911221309627.png)

```bash
#getadd 添加地理位置
#规则： 两级无法直接添加，我们一般会下载城市数据，通过java程序一次性导入
#有效的经度-180到180
#有效的维度 -85.05112878到85.05112878度
#当坐标位置超出这个范围的时候，该命令将会返回一个错误
#127.0.0.1:6379 > geoadd china :city 39.90.116.40 北京
(error) ERR invalid longitude ,latitude pair 39.900000,116.400000

#参数key  值()
geoadd china:city 116.40 39.90 beijing
geoadd china:city 121.40 31.90 shanghai
```



#### getpos

![image-20250911222352905](./redis.assets/image-20250911222352905.png)

```bash
getpos china:city beijing #获取指定的城市的经度维度
```



#### getdist

![image-20250911222846056](./redis.assets/image-20250911222846056.png)

+ m 表示单位米
+ km表示单位为千米
+ mi表示单位为英里
+ ft表示单位为英尺

```bash
geodist china:city beijing shanghai km #查看上海到北京的直线距离
geodist china:city beijing chongqin km # 查看重庆到北京的直线距离
```



#### georadius  

`以给定的经纬度为中心，找出某一半径内的元素`

我附近的人/(获取所有的附近的人的的地址定位!) 通过半径来查询！

获取指定的数量的人：200

所有的数据都应该录入:china:city ,  结果才会显示这个查询到的内容

![image-20250911224536044](./redis.assets/image-20250911224536044.png)

![image-20250911225024447](./redis.assets/image-20250911225024447.png)

```bash
georadius china:city 110 30 1000 km #以110，30 这个经纬度为中心，寻找方圆1000km内的城市
georadius china:city 110 30 500 km withdist #显示到中间的位置
georadius china:city 110 30 500 km withcoord #显示他人位置
georadius china:city 110 30 500 km withdist withcoord count #筛选出指定的结果!
```

#### georadiusbymember  

查找指定经纬度周围的元素

![image-20250911225427291](./redis.assets/image-20250911225427291.png)

```bash
#找出位于指定元素周围的其他元素！
georadiusbymember china:city beijing 1000 km
georadiusbymember china:city beijing 400 km
```



#### geohash 

返回一个或多个位置的geohash表示

![image-20250911225907674](./redis.assets/image-20250911225907674.png)

该命令将返回一个11位的字符串

```bash
geohash china:city beijnig chongqin #将二维的经纬度转换为一个一维的字符串，如果两个字符串越接近，那么距离就越近
```

#### geo底层的实现原理

其实就是Zset！我们可以使用Zset命令来操作geo

![image-20250911230521885](./redis.assets/image-20250911230521885.png)

```bash
zrange china:city 0 -1 #查询地图的全部元素
zrem china:city  beijing # 移除元素
```



### Hyperloglog

#### 什么是基数

A{1,2,3,2,5,7,8}

B{1,2,3,5,8,7}

基数（不重复的元素）=9，可以接收误差！

#### 简介

redis 2.8.9版本更新了hyperloglog数据结构！

redis hyperloglog 基数统计的算法！

优点：占用的内存是固定的，2^64不同的元素的计数，只需要废12KB的内存！如果要从内存角度来比较的话hyperloglog首选！

**网页的UV（一个人访问一个网站多次，但是还是算作一个人！）**

传统的方式，set保存用户的id，然后就可以统计set中的元素数量作为标准判断！

这个方式如果保存大量的用户id，就会比较麻烦！我们的目的是为了计数，而不是保存用户id；

0.81%错误率！统计UV任务，可以忽略不计的！

#### 常规操作

![image-20250911234519427](./redis.assets/image-20250911234519427.png)

```bash
pfadd key value value vlaue # 创建第一组元素 key
pfcount key #统计元素个数
pfadd key2 a a a a as f g h j k #创建第二组元素
pfmerge key3 key key2 #合并两组 key + key2 - >  key3 并集
pfcount key3  # 查寻刚刚并集的数量
```

如果允许容错一可以使用hyperloglog

### bitmap

#### 位存储

统计用户信息，活跃，不活跃！登录、未登录！打卡，365打卡！两个状态的，都可以使用bitmaps！

bitmaps位图，数据结构！都是二进制位来进行记录，就只有0和1两个状态！

365天=365bit 

1字节 kb=  8bit   

暂用 46字节(kb)

```bash 
 1 bit（位） = 一个二进制数字（0或1）
 1 Byte（字节） = 8 bits
 1 KB（Kilobyte） = 1024 Bytes =2^10
 1 MB（Megabyte） = 1024 KB = 2^10 KB
 1 GB（Gigabyte） = 1024 MB = 2^10MB
 1 TB（Terabyte） = 1024 GB = 2^10GB
```

位图:

![image-20250912204514871](./redis.assets/image-20250912204514871.png)

就是这样一直排序下去保存`0or1`

使用`bitmap`来记录周一到周日的打卡！

周一：1 周二：0 周三：0 周四：1 .....

![image-20250912205602420](./redis.assets/image-20250912205602420.png)

```bash
setbit sign 1 0 # 设置周一未打卡
setbit sign 2 1 # 设置周二打卡了
```



查看某一天是否有打卡

![image-20250912210104128](./redis.assets/image-20250912210104128.png)

```bash
bitcount sign #统这周的打卡记录，就可以看见是否全勤！
```











## 事务

#### 事务的本质

本质：一组命令的集合！一个事务中的所有的命令都会被序列化，在事务执行的过程中，会按照顺序执行！

**一次性、顺序性、排他性！执行一些列命令！**

```bash
---------队列 set set set 执行 --------

```
==redis单挑命令是保证原子性的，但是事务不保证原子性!==

==redis事务没有隔离级别的概念！==

所有的命令在事务中，并没有直接被执行！只有**发起执行命令的时候才会执行**！

redis的事务：

+ 开启事务（multi）
+ 命令入队（.........）
+ 执行事务（exec）

#### 正常执行事务

![image-20250912212404980](./redis.assets/image-20250912212404980.png)

```bash
multi   #   开启事务

#命令入队
set k1 v1 
set k2 v2
exec    #  执行事务
```

#### 放弃事务

![image-20250912212946278](./redis.assets/image-20250912212946278.png)

```bash
multi #开启事务
...
...
discard #开启事务
```

#### 编译型异常

代码有问题！命令错误，，事务中所有的命令都不会执行

![image-20250912213601737](./redis.assets/image-20250912213601737.png)

#### 运行时异常

如果存在事务队列中存在语法性，那么执行命令的时候，其他命令是可以正常执行的，错误命令抛出异常！

![image-20250912214306530](./redis.assets/image-20250912214306530.png)

````bash
set k1 "v1"   #  设置一个字符串的值
multi         # 开启事务
incr k1       # 会执行失败
set k2 v2  
set k3 v3 
get k3 
get k2
exec    #结束事务
# 第一条事务会执行失败但是其他的会成功
# redis的事务不能够保证原子性，所以会出现这种情况
````

#### 监控 watch

悲观锁：

+ 很悲观，什么时候都会出现问题，无论做什么都会加锁

乐观锁：

+ 很乐观，认为什么时候都不会出现问题，所以不会上锁，更新数据的时候需要取判断一下，在此期间是否有人修改过这个数据，version！
+ 获取version
+ 更新的时候比较version

这些是mysql里面的

#### redis测试监视

##### 正常执行

![image-20250912221452348](./redis.assets/image-20250912221452348.png)

```bash
watch money #监视money 对象
mutli #事务正常结束，数据期间没有发送变动，这个时候就正常执行成功！
....
exec
```

##### 测试多线程修改值

使用watch可以当做redis的乐观锁操作！

![image-20250912234015717](./redis.assets/image-20250912234015717.png)

```bash
watch money # 监视 money
...
...
exec # 执行之前 ，另一个线程修改了值，这个时候就会导致事务的执行失败，就需要解锁
unwatch #失败后解锁
```

##### 修改失败对应的操作

![image-20250912234223964](./redis.assets/image-20250912234223964.png)

```bash
# 1.发现事务执行失败，先进行解锁操作
unwatch
# 2.获取最新的值，再次监视，select version 
watch money 
multi 
decrby money 1
decrby money 1
exec 
# 3 比较监视的值是否发生了变化，如果没有变化，那么可以执行成功，如果有变化，那么就继续重复
```









## jedis



纯纯和上面的命令一致，不展示，而且也不用这个





## springboot整合

jedis：springboot2.x之后，原来使用的jedis被替换为了lettuce

jedis：采用的直接连接，多个线程操作的话，是不安全的，如果想要避免不安全的话，使用jedis pool 连接池 类似于bio模式

lettuce： 采用netty，实例可以再多个线程中进行共享，不存在线程不安全的情况！可以减少线程数据了，更像nio模式

![image-20250913105340385](./redis.assets/image-20250913105340385.png)





## redis.conf详解

### 配置大小

![image-20250913141020055](./redis.assets/image-20250913141020055.png)

配置文件堆大小不敏感

### 包含

可以包含别的配置文件

![image-20250913141134036](./redis.assets/image-20250913141134036.png)

### 网络配置

```bash
bind 127.0.01 #绑定的ip
protected-mode yes #保护模式 
prt 6379 #端口设置
```

### 通用general

```bash
daemonize yes #以守护进程的方式运行，默认是no ,我们需要自己开启为yes！

pidfile /var/run/redis_6379.pid # 如果以后台的方式运行就需要指定一个pid文件
```

**日志**

![image-20250913142059928](./redis.assets/image-20250913142059928.png)

```bash
# 多种隔离级别可选
loglevel notice 
logfile "" #日志的文件位置 如果为空，那就是直接输出打印
databases 16  # 数据库的数量 默认16
always-show-logo yes #是否总数显示logo
```

### 快照

持久化，再规定的时间内，执行了多少次操作，则会持久化到文件.rdb/.aof

redis是内存数据库，如果没有持久化，那么数据断电就会丢失！

![image-20250913145859837](./redis.assets/image-20250913145859837.png)

```bash
#如果900 秒内，至少有一个key进行了修改，我们就会进行持久化（换而言之就是控制持久化的时间间隔，根据数量来控制）
save 900 1
#如果300秒内，至少有10个key进行修改，我们及进行持久化操作
svae 300 10
#如果60s内，至少有10000个key进行了修改，我们及进行持久化操作
save 60 10000
# 支持自定义

stop-writes-on-bgsave-error yes #如果持久化出现错误，是否还要继续工作！
rdbcompression yes #是否压缩rdb文件，需要消耗一些cpu资源！
rdbchecksum yes #保存rdb文件的时候，进行错误的检查校验！
dir ./ # rdb文件保存的目录
```



### replication 复制，后面主从复制讲解

### security安全

可以设置redis的密码，默认是没有密码的！

![image-20250913151049839](./redis.assets/image-20250913151049839.png)

```bash
config get requirepass #获取redis的密码
config set requirepass  "123456" # 设置redis的密码
auth 123456 #验证权限
```

### 客户端限制

![image-20250913152620035](./redis.assets/image-20250913152620035.png)

```bash
maxclients 1000 #设置能连接上的redis的最大客户端的数量
maxmemory <bytes> # redis配置最大的内存容量
maxmemory-policy noeviction #内存达到上限后的处理策略
        1.volatile-lru : 只对设置了过期时间的key进行lru（默认值）
        2.allkeys-lru: 删除lru算法的key
        3.volatile-random ：随机删除即将过期key
        4.allkeys-random ： 随机删除
        5.volatile-ttl ： 删除即将过期的
        6.noeviction ： 永不过期，返回错误
        
        # volatile 是值设置了过期时间的key
        # allkeys是值得所有key
```

### append only 模式 aof配置

![image-20250913154116397](./redis.assets/image-20250913154116397.png)

```bash
appendonly no #默认是不开启aof模式的默认使用的是rdb持久化的机制，在但部分的情况下，rdb完全是够用的！
appendfilename "appendonly.aof" #持久化文件的名字

# appendfsync always #每次修改都会sync 。消耗性能
appendfsync everysec # 每秒执行一次sync，可能会丢失这一秒的数据
# appendsync no #不执行sync，这个时候操作系统自己来同步数据，速度是最快的

```



## redis持久化机制

### RDB

在主从复制中，rdb就是备用的，在从机上面，不占用主机的空间

redis是内存数据库，如果不将内存中的数据库状态保存到磁盘中，那么一旦服务器进程退出，服务器中的数据库状态就都会消失。所以redis提供了持久化功能！

![image-20250913164306985](./redis.assets/image-20250913164306985.png)

在指定的时间间隔将内存中的数据集快照写入磁盘，也就是行话说的snapshot快照，他恢复时是将快照文件直接读内存里面。

redis会单独创建一个子线程（fork），用于持久化，会先将数据写入一个临时的文件中，带持久化过程都结束了，在将临时文件替换掉上次持久化的文件。在整个过程中，主进程是不会进行任何的IO操作的 ，这就确保了极高的性能，如果需要进行大规模的恢复，且对于数据恢复的完整性不是很敏感，那rdb方式要比aof方式更加高效。rdb的缺点是==最后一次持久化后的数据可能会丢失==。我们默认使用的就是rdb，一般情况下不会修改这个配置！

rdb保存的文件是dump.rdb   一般都是在配置文件中进行设置

![image-20250913170558110](./redis.assets/image-20250913170558110.png)

![image-20250913170618735](./redis.assets/image-20250913170618735.png)

#### 触发机制

1. save的规则满足的情况下，会自动触发rdb规则
2. 执行flushall命令，也会触发rdb规则
3. 退出redis，也会产生rdb文件！

备份就自动生成一个dump.rdb
![image-20250913171003214](./redis.assets/image-20250913171003214.png)

#### 如何恢复rdb文件

1. 只需要将rdb文件放到我们对应的redis启动目录下就可以了，redis启动的时候会检查dump.rdb并恢复其中的数据
2. 查看需要存在的位置

```bash
config get dir
"dir"
"/uer/local/bin"   #如果在这个目录下面存在dump.rdb文件，启动的时候就会自动的恢复其中的数据
```

**优点：**

+ 适合大规模的数据恢复
+ 对数据的完整性要求不高

**缺点：**

+ 需要一定时间间隔进程操作！如果redis意外宕机了，这个最后一次修改的数据就没有了
+ fork进程的时候，会占用一定的内存空间！！

### AOF

将所有的命令记录下来，相当于history，恢复的时候相当于把文件全部执行一遍

#### 是什么

![image-20250913175516199](./redis.assets/image-20250913175516199.png)

以日志的形式来记录每一个写操作，将redis执行过的所有的指令记录下来（只记录写操作，读不用管），只许追加文件但不可以改写文件，redis启动之初回读取该文件重新构建数据，换而言之，redis重启的话就根据日志文件的内容将指令从前执行到后执行一次完成数据的恢复工作

aof保存的是appendonly.apf文件

#### append

![	，](./redis.assets/image-20250913180313412.png)

默认不开启需要手动的配置，改为yes为开启，

重启，redis就可以生效！

如果这个aof文件有问题，这个时候redis是连接不了的

使用==redis-check-aof --fix==命令俩修复文件

![image-20250913200041982](./redis.assets/image-20250913200041982.png)

如果文件正常，重启就可以自己恢复了！

![image-20250913200128411](./redis.assets/image-20250913200128411.png)



---

策略：

![image-20250913180420222](./redis.assets/image-20250913180420222.png)

```bash
appendfsync always #每次出现写入操作后就将这个命令写入sof
appendfsync everysec  # 每一秒执行一次写入
appendfsync no # 不需要同步写入aof，关闭
```

重写

![image-20250913191400143](./redis.assets/image-20250913191400143.png)

从写的时候是否使用appendfsync

默认为no

---

#### 重写机制

aof默认就是文件的无限制追加，文件就会越来越大

![image-20250913200737866](./redis.assets/image-20250913200737866.png)

如果aof文件的大小超过64mb，就会fork一个新的进程来将我们的文件进行重写

---

#### 优点与缺点

**优点**

+ 每一次修改都同步，文件的完整性会更好！
+ 每秒同步一次，可能会丢失一秒的数据（最后一秒）
+ 从同步数据，效率最高的！

**缺点**

+ 相对于数据文件来说，aof远远大于rdb，修复的速度也比rdb慢！
+ aof运行效率也要比rdb慢，所以我们redis默认的配置就是rdb持久化

### 扩展

1. RDB持久化方式能够在指定的时间间隔内对你的数据进行快照存储
2. AOF持久化记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以redis协议追加保存每次写的操作到文件末尾，redis还能对AOF文件进行后台重写，是的AOF文件体积不至于过大。
3. ==只做缓存，如果你只希望你的数据在服务器上运行的时候存在，你也可以不进行任何的持久化==
4. 同时开启 两种持久化方式
    + 在这种情况下,当redis重启的时候会优先载入AOF文件来恢复原始的数据,因为在通常情况下AOF保存的数据集要比RDB文件保存的数据集要完整.
    + RDB的数据不实时,同时使用两者时服务器重启也只会找AOF文件,那要不要使用AOF呢?一般不用,因为RDB更适合用于备份数据库库(AOF在不断变换不好备份),快速重启,而且不会有AOF可能潜在的bug,留着作为一个万一的手段
5. 性能建议
    + 因为RDB文件只作为后备用途，建议只在Slave持久化RDB文件。而且只要15分钟备份一次就够了，只保留save 900 1 这条规则
    + 如果Enable AOF ，好处是在最恶劣的情况下也只会丢失不超过两秒的数据，启动脚本较简单只load自己的AOF文件就可以了，代价
      + 一： 带来持续的IO
      + 二：AOF rewrite 的最后将rewrite 过程中产生额新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite 的频率，AOF 重写的基础大小默认值64MB太小了，可以设置到5G左右，默认超过元大小！100%大小重写可以改到适当的数值
    + 如果不Enable AOF ，仅靠Mater-Slave Repllcation 实现高可用也可以，能省掉一大笔IO,也减少了rewrite 时带来的系统波动，代价是如果Master/Slave同时倒掉，会丢失十几分的数据，启动脚本也要比较两个Master/Slave中的RDB文件，载入新的哪一个，微博就是这种架构.

---

## redis发布订阅

redis发布订阅（pub/sub）是一种==消息通信模式==： 发送者（pub）发送消息，订阅者（sub）接收消息.微信、微博、关注系统！

redis客户端可以订阅任意数量的消息

订阅/发布消息图：

第一个：消息发送者

第二个：频道

第三个:消息订阅者

![image-20250913203428488](./redis.assets/image-20250913203428488.png)

下图展示了 频道channel1，以及订阅了这个频道的三个客户端--client2 、 client5 和client1 之间的关系：

![image-20250913203955989](./redis.assets/image-20250913203955989.png)

当有新消息通过publish 命令发送给频道channel时，这个消息就会发送给订阅他的三个客户端：

![image-20250913204109447](./redis.assets/image-20250913204109447.png)





### 常用的命令

这些命令被广泛的应用于即时通信应用，比如网络聊天室和实时广播、实时提醒等。

![image-20250913204206845](./redis.assets/image-20250913204206845.png)

### 测试

#### 订阅端

![image-20250913204918519](./redis.assets/image-20250913204918519.png)

订阅命令：

```bash
subscribe kuangshenshuo #订阅命令  
```



发送端：

![image-20250913204935779](./redis.assets/image-20250913204935779.png)

```bash
publish kuangshnesho #向指定的频道推送消息
```



### 原理

![image-20250913213631666](./redis.assets/image-20250913213631666.png)

![image-20250913213651290](./redis.assets/image-20250913213651290.png)

使用场景：

+ 实时消息系统！
+ 实时聊天！（频道当做聊天室，将消息回显给所有的人即可！）
+ 订阅，关注系统都是可以的！

稍微复杂的场景我们就会使用消息中间件 MQ

## redis主从复制

### 概念

主从复制，是指将一台redis服务器的数据，复制到其他的redis服务器上。前者称为主节点Master/leader，后者成为从节点Slave/follower，数据的复制是单项的，只能由主节点到从节点。Master 一写为主，Slave 以读为主.

**默认情况下，每台redis服务器都是主节点；**且一个主节点可以有**多个从节点(或者没有从节点)**,但一个从节点只能有一个主节点。

主从复制的作用主要包括：

1. **数据冗余**: 主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
2. **故障恢复**：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复，实际上是一种冗余服务
3. **负载均衡**：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写redis数据时应用连接主节点，读redis数据连接从节点）分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提升redis服务器的并发量。
4. **高可用（集群）基石**：除了上述作用以外，主从复制还是哨兵模式和集群能够实施的基础，因此说主从复制的redis高可用的基础。



一般来说，要将redis运用于项目中，只使用一台redis是万万不能的（宕机，一主二从），原因如下:

1. 从结构上来说，单个redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大；
2. 从容量上，大哥redis服务器内存容量有限，就算一台redis服务器的内存是256G，也不能够将所有的内存用于redis存储服务，
3. 一般来说一台redis使用的内存不应该超过20G

电商网站上的上商品，一般都是一次上传，多次浏览，即==读多写少==

![image-20250913215938359](./redis.assets/image-20250913215938359.png)

主从复制，读写分离！80%的情况下都是在惊醒读操作！减缓服务器的压力！架构中经常使用！ 一主二从！

只要在公司中，主从复制就算必须使用的，应为真实的项目中不可能单机使用redis！

### 环境配置

只需要配置从库，不用配置主库！

![image-20250913221250535](./redis.assets/image-20250913221250535.png)

```bash
info replication #查看当前库的信息
```

复制3个配置文件，然后修改对应的信息

1. 端口
2. pid 名字
3.  log 文件名字
4. dump.rdb名字

修改完之后，启动我们的3个redis服务器，可以通过进程信息查询！

![image-20250913222317001](./redis.assets/image-20250913222317001.png)

### 一主二从

默认的情况下，每一台redis服务器都是主节点；我们一般情况下只用配置从机就好了！

认老大！一主（79） 二从（80，81）

![image-20250913222746375](./redis.assets/image-20250913222746375.png)

```bash
slaveof 127.0.0.1 6379 #在从机上配置的主机
```

真实的配置应该在配置文件里面去配置

### 细节

主机可以写，从机不能写只能读！主机中的所有信息和数据，都会自动被从机保存！

主机写：![image-20250913223907674](./redis.assets/image-20250913223907674.png)

从机只能读取内容！

![image-20250913224006029](./redis.assets/image-20250913224006029.png)

测试：主机断开连接，从机依旧连接到主机的，但是没有写操作，这个时候，如果主机回来了，从机依旧可以直接获取主机写的信息！

如果是使用命令行，来配置的主从，这个时候如果重启了，就会变回主机！只要变为主机，立马就会获取主机中的值！

### 复制原理

Slave启动成功连接到master后会发送一个sync命令

Master接收到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，==master将传送整个数据文件到slave，并完成一次完成同步==。

全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

增量复制：Master继续将新的所有收集到的修改命令一次传给slave，完成同步。

但是只要重新连接master，一次完全同步（全量复制）将被自动执行！我们的数据在从机中就一定能够找到

### 层层链路  

上一个M连接下一个S!

![image-20250913225934260](./redis.assets/image-20250913225934260.png)

这个时候也可以完成主从复制，但是80任然是从节点

> 如果没有了老大，这个时候能不能手动选择一个老大呢？可以手动

==谋朝篡位==

如果主机断开了连接，我们可以使用slaveof no one 让自己变成主机！其他的节点就可以手动连接到最新的这个主节点（手动）！如果这个时候老大修复了，那就重新连接！（就算他不会在成为主节点需要手动配置了）

## 哨兵模式

（自动选举老大版）

### 概述

主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内的服务不可用。这就是一种不推荐的方式，更多的时候我们会优先考虑哨兵模式。redis从2.8开始正式提供了sentinel(哨兵模式)架构来解决这个问题。

谋朝篡位自动版，能够后台监控主机是否故障，如果故障了根据投票数==自动将从库转换为主库==。

哨兵模式是一种特殊的模式，首先redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，他会独立运行，其原理是哨兵**通过发送命令，等待redis服务器响应，从而监控运行的多个redis实例**。

![image-20250913234545726](./redis.assets/image-20250913234545726.png)

这里的哨兵的作用

+ 通过发送命令，让redis服务器返回监控状态，包括主服务器和从服务器
+ 当哨兵模式检测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他服务器，修改配置文件，让他们切换主机。

然而一个哨兵进程对redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控，各个哨兵之间还会进行监控，这样据形成了多哨兵模式。

![image-20250913235100564](./redis.assets/image-20250913235100564.png)

假设主服务器宕机哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观认为主服务器不可用，这个现象称为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量到达一半时，那么哨兵之间开始投票，选取一个主哨兵，进行failover[故障转移]操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的服务器实现切换主机，这个过程称为**客观下线**

### 测试

我们目前的状态是一主二从！

1、配置哨兵配置文件sentinel.conf

```bash
# sentinel monitor 被监控的名称 host port 1
sentinel monitor myredis 127.0.0.1 6379 1
```

后面的这个数字1，代表主机挂了，slave投票看让谁替成主机，票数最多的，就会成为主机！

2、启动哨兵

![image-20250914000529102](./redis.assets/image-20250914000529102.png)

如果主节点断开了，这个时候就会从从机中随机选择一个服务器！（在里面有一个算法！）

![image-20250914000709235](./redis.assets/image-20250914000709235.png)

**优点**

+ 哨兵集群，基于主从复制模式，所有的主从配置优点，他全有
+ 主从可以切换，故障可以转移，系统的可用性就会更好
+ 哨兵模式就算主从模式的升级，手动到自动，更加健壮

**缺点**

+ redis不好在线扩容，集群的容量一定到达上限，在线扩容就会十分的麻烦
+ 实现哨兵模式的配置其实很麻烦，里面由很多选择！

### 哨兵模式的全部配置

![image-20250914001552573](./redis.assets/image-20250914001552573.png)

![image-20250914001645198](./redis.assets/image-20250914001645198.png)

![image-20250914001728781](./redis.assets/image-20250914001728781.png)

















## redis缓存穿透、击穿和雪崩

### 缓存穿透

#### 概念

用户想要查询一个数据，发现redis内存数据库中没有，也就缓存没有命中，于是向持久层数据库发起查询请求，持久层数据库也没有，于是就会出现查询失败的结果，当有大量的用户访问的时候，数据库就会崩溃。

简：大量的访问redis，数据库中没有的数据，导致数据库崩溃

==也可以理解为redis中没有，直接就打到了数据库，数据库也没有==

![image-20250914130503633](./redis.assets/image-20250914130503633.png)

#### 产生的原因

+ 参数有误，所以redis以及数据库根本就没有这个数据
+ redis以及数据库没有

#### 解决方案

1. 设置key为null

> 在第一次查到之后将他放到redis里面就可以，返回一个null
>
> 这样就避免了大量的请求去访问mysql导致其崩溃

![image-20250914131308967](./redis.assets/image-20250914131308967.png)



2. 布隆过滤器

![image-20250914131139676](./redis.assets/image-20250914131139676.png)

> 一般情况都会进行参数校验所以这里不作为解决方案



### 缓存穿透

#### 概念

某一个热点key在缓存中过期（或者根本就不存在），但是这时候出现了大量的请求访问该数据，导致这些请求打到了数据库

举个栗子：

"某过气男网红xx未遂，xxxxx"这种数据，一般是无人在意的突然上了热搜，导致大量的人访问

#### 产生的原因

**redis中没有对应的数据或者数据已过期**

#### 解决方案

+ key永不过期：

  已经是热点数据的key永不过期，在我们的后台使用定时任务来对他进行更新

+ 互斥锁：

  因为是多个线程同时对一个数据进行访问，所以可以使用互斥锁，只放一个线程进去拿到结果，然后放到redis里面。

+ 缓存预热：

  在系统启动初期或者低峰期，进行预热，将数据放入缓存中

### 缓存雪崩

#### 概念

某一个时间段内，**大量缓存**集中过期失效，redis中**没有对应的数据**，这时候**大量的请求**就会打到数据库，数据库over

![image-20250914140545442](./redis.assets/image-20250914140545442.png)

#### 产生原因

大量的请求访问大量的不存在于redis的数据

#### 解决方案

+ **redis高可用**

顾名思义，redis既然可能挂掉，就扩展redis集群添加对应的redis的数量

+ **限流降级**

在缓存失效后，通过加锁或者消息队列来控制数据库写缓存线程数量，比如对某个key只允许一个线程查询数据和写缓存，其他线程等待

+ **数据预热**
在系统启动初期或者低峰期，进行预热，将数据放入缓存中



> redis在这时候的作用的当做缓存，是为了分担数据库的压力，所以一旦有大量的请求达到数据库，数据库崩溃进而整个服务崩溃，在设计的时候可以往这方面靠，避免因为yigekey而导致整个程序over
