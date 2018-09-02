---
layout: post
title: 分布式之RedisCluster知识点总结
date: 2018-07-22
tags: [RedisCluster 高并发 性能优化 分布式锁设计]
---
## 基础部分 ##
### 1.Redis 数据类型分析 ###
[doc](http://redisdoc.com/){:target="_blank"}
1. String
>set/get 批量存取；失效expire机制；失效后出发级联动作
作用 1）全局技术 2）缓存一般值
数据结构：
2. Hash
>hset/hget hmset/hmget 存取结构化数据，比较方便的就是操作其中的某个字段；
1）单点登录 2）key filed value
数据结构：
3. List
>lpush/lpushx/lpop
作用 1）简单消息队列  2）分页 lrange
数据结构：
4. Set
>sadd/spop/smembers
作用 1）全局去重的功能 2）利用交集、并集、差集等操作，可以计算共同喜好，全部的喜好，自己独有的喜好等功能
数据结构：
5. Sorted Set
>sorted set多了一个权重参数score,集合中的元素能够按score进行排列
作用 1）排行榜应用，取TOP N操作 2)sorted set可以用来做延时任务 3)范围查找
数据结构：

### Redis 内存估算 ###
[内存模型](http://wadeling.blogspot.com/2016/07/redis.html){:target="_blank"}
redis占用的内存除了业务数据之外，还有redis自己的数据结构也会占用一部分内存，所以实际占用内存会大于业务数据的
>例子： 5000万key，每个key40字节（string类型），每个value8字节
1.首先是计算公式:
string类型的内存大小 = 键值个数 * (dictEntry大小 + redisObject大小 + 包装key的sds大小 + 包装value的sds大小) + bucket个数 * 4
2.根据使用方提供的数据，占用的redis内存容量如下：
dictEntry大小: 16 字节
redisObject大小：16字节
key:40字节, sds对象:9 字节。总共是49字节，按jemalloc分配机制会占用到64字节。
value：8字节，sds对象：9字节，总共是17字节，按jemalloc分配机制会占用到32字节。
以上的内存占用量为：
（16 + 16 + 64 + 32）* 5000*1000*10 = 6.4G
bucket个数大概为6000万（5000万向上取整到2的n次方，即2的26次方，6000多万）
bucket大小：6000万*4 = 240M左右。
 
[REDIS内存容量的预估和优化](https://searchdatabase.techtarget.com.cn/7-20218/){:target="_blank"}
[内存管理算法总结:ptmalloc、tcmalloc和jemalloc](https://blog.csdn.net/junlon2006/article/details/77854898/){:target="_blnak"}
### Redis 主要解决的问题 ###
1. 性能
a) 要求迅速响应的接口，且结果不频繁变动的SQL结果集缓存。(不经常变动、定时过期 )
2. 并发
b) 高并发情况下，所有的请求直接访问数据库，数据库会出现连接异常。(广告位判定是否存在)
### Redis集群的冷热数据分离 
### Redis分布式并发问题/事务应用
## 高级特性 ##
### Redis 优化(Codis&RedisCluster;多端口实例；参数调优) ###
### Redis 要考虑的问题
(一)缓存和数据库双写一致性问题
(二)缓存雪崩问题
(三)缓存击穿问题
(四)缓存的并发竞争问题
(五)过期策略触发某个动作；内存淘汰机制