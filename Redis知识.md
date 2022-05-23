# Redis入门

## Redis概述

> Redis是什么？

Remote Dictionary Server，即远程字典服务！是一个开源的Key-Value数据库

> Redis能干嘛？

1. 内存存储、持久化，内存中是断电即失，所以说持久化很重要（rdb、aof)
2. 效率高，可以用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器

## Linux安装

1. 下载安装包

2. Linus中解压缩

3. 进入解压后的文件，可以看到我们redis配置文件

   ![image-20220519235139608](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220519235139608.png)

4. 基本的环境安装

   ```bash
   yum install gcc-c++
   
   make
   ```

5. redis默认安装路径

   ![image-20220520001529002](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520001529002.png)

6. 将redis配置文件，拷贝至当前目录中的配置目录（自己创建的llqconfig，以后都操作这个配置文件，不操作原本的配置文件，便于保存原来的副本）

```bash
cp /raid/llq/redis-stable/redis.conf ./llqconfig/
```

7. redis默认不是后台启动的，修改配置文件！(改为yes)

   ![image-20220520002322590](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520002322590.png)

8. 启动redis服务（通过指定的配置文件）

   ![image-20220520003000920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520003000920.png)

9. 使用redis-cli进行连接

![image-20220520003106998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520003106998.png)

10. 查看redis的进程是否开启

    ![image-20220520004757690](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520004757690.png)

11.如何关闭redis服务

![image-20220520005134971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520005134971.png)

## 性能测试

redis-benchmark是一个压力测试工具！

官方自带的性能测试工具！下图来自菜鸟教程

![image-20220520005430984](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520005430984.png)

## 基础知识

redis默认有16个数据库

![image-20220520005953399](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220520005953399.png)

默认使用第0个

可以使用select进行切换数据库，flushdb清楚当前数据库，flushall清楚所有数据库

```bash
127.0.0.1:6379> select 3 切换到第三个数据库
OK
127.0.0.1:6379[3]> dbsize 查看数据库大小
(integer) 0
127.0.0.1:6379[3]> keys * 查看所有key
(empty array)
127.0.0.1:6379[3]> set name fym 设置key=name， valuefym
OK
127.0.0.1:6379[3]> get name 查看key
"fym"
127.0.0.1:6379[3]> keys *
1) "name"
127.0.0.1:6379[3]> flushdb 清除当前数据库
OK
127.0.0.1:6379[3]> keys *
(empty array)

```

> Redis是单线程的

官方辨识，redis是基于内存操作，CPU不是redis性能瓶颈，redis的瓶颈是根据机器的内存和网络带宽，既然可以使用单线程来实现，就是用单线程

**redis为什么单线程还这么快**？

1.误区1： 高性能的服务器一定是多线程的？

2.误区2：多线程一定比单线程效率高？

核心：redis是将所有的数据全部放在内存中，所以说使用单线程去操作效率 最高的，多线程（cpu上下文会切换，比较耗时），对于内存系统来说，如果没有上下文切换效率是最高的，多次读写都是在一个CPU上的。



##  五大基本数据类型

### Redis-Key

```bash
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name llq
OK
127.0.0.1:6379> type name #查看当前key的类型
string
127.0.0.1:6379> EXISTS name #查看当前key是否存在
(integer) 1
127.0.0.1:6379> EXISTS name1
(integer) 0
127.0.0.1:6379> EXPIRE name 10 #设置当前key过期时间，时间是秒
(integer) 1
127.0.0.1:6379> ttl name #查看当前key剩余存在时间
(integer) 6
127.0.0.1:6379> ttl name
(integer) 5
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name #当前key已经不存在了
(nil)
```

### String

```bash
127.0.0.1:6379> GET name #获得值
"llq"
127.0.0.1:6379> APPEND name "fym" #追加值，如果不存在这个key，相当于set key,
(integer) 6
127.0.0.1:6379> GET name
"llqfym"
127.0.0.1:6379> STRLEN name #返回当前key对应的value的字符串的长度
(integer) 6

##################################################
127.0.0.1:6379> SET views 0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> INCR views #+1
(integer) 1
127.0.0.1:6379> get views
"1"
127.0.0.1:6379> DECR views #-1
(integer) 0
127.0.0.1:6379> INCRBY views 10 #+10
(integer) 10
127.0.0.1:6379> DECRBY views 15 #-15
(integer) -5
###################################################
#字符串范围range
127.0.0.1:6379> get name
"llqfym"
127.0.0.1:6379> GETRANGE name 0 3 #截取字符串[0,3]
"llqf"
127.0.0.1:6379> GETRANGE name 0 -1 #截取全部字符串
"llqfym"
127.0.0.1:6379> SETRANGE name 1 xx #替换指定位置开始的字符串
(integer) 6
127.0.0.1:6379> get name
"lxxfym"
###################################################
#setex(set with expire) #设置过期时间
#setnx(set if not exist) #不存在该key才设置(在分布式锁中会常常使用)
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name llqfym
OK
127.0.0.1:6379> get name
"llqfym"
127.0.0.1:6379> set key3 30 hello #30s后过期
(error) ERR syntax error
127.0.0.1:6379> setex key3 30 hello
OK
127.0.0.1:6379> ttl key3
(integer) 22
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey "redis" #不存在mykey时才设置
(integer) 1
127.0.0.1:6379> keys *
1) "mykey"
2) "name"
127.0.0.1:6379> setnx mykey "MongDB" #设置失败，因为已经存在mykey
(integer) 0
127.0.0.1:6379> get mykey
"redis"
###################################################
#同时设置多个值 & 同时获取多个值
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3 #同时设置多个值
OK
127.0.0.1:6379> keys *
1) "mykey"
2) "name"
3) "k2"
4) "k3"
5) "k1"
127.0.0.1:6379> mget k1 k2 k3 #同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4 #msetnx是一个原子性的操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379> get key4
(nil)

###################################################


127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2 #这里的key是一个巧妙的设计，user:{id}:{filed},如此设计在redis中是ok的
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"
127.0.0.1:6379> set user:2 {name:lisi,age:4} #设置一个user:2对象，值为json字符串来保存
OK
127.0.0.1:6379> mget user:2
1) "{name:lisi,age:4}"
127.0.0.1:6379> keys * #看看当前的keys
1) "mykey"
2) "user:2"
3) "user:1:age"
4) "name"
5) "user:1:name"
6) "k2"
7) "k3"
8) "k1"

###################################################
#getset,先get再set
127.0.0.1:6379> getset db "redis"#如果不存在，返回nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db "mongdb"#如果存在值，先获取原来的值，然后设置新的值
"redis"
127.0.0.1:6379> get db
"mongdb"
```

### List

在redis里面，我们可以把list完成栈、队列、阻塞队列

所有的list命令都是用L开头的

```bash
127.0.0.1:6379> LPUSH list one #将
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LRANGE list 0 1 
4) "three"
5) "two"
```



