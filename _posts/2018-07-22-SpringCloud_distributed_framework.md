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

* Redis集群的冷热数据分离 
* Redis 分布式并发问题/Redis 事务应用


### Redis 内存估算 ###
[内存模型](http://wadeling.blogspot.com/2016/07/redis.html){:target="_blank"}
### Redis 主要解决的问题 ###
性能
a) 执行耗时特别久，且结果不频繁变动的SQL结果集缓存。(不经常变动、定时过期  广告物料信息，用户登录信息，定向投放条件)
并发
b) 高并发情况下，所有的请求直接访问数据库，数据库会出现连接异常。(广告位判定是否存在)
## 高级特性 ##
### Redis 优化(Codis&RedisCluster;多端口实例；参数调优) ###
### Redis 注意事项(缓存和数据库双写一致性问题；缓存的并发竞争问题；过期策略触发某个动作；内存淘汰机制；应对缓存击穿和缓存雪崩) ###