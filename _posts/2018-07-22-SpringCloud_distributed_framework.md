---
layout: post
title: 分布式之RedisCluster知识点总结
date: 2018-07-22
tags: [RedisCluster 高并发 性能优化 分布式锁设计]
---
## 概述 ##
### Redis 数据类型及底层数据结构&数据类型使用场景 ###
![doc](http://redisdoc.com/){:target="_blank"}


Redis集群的冷热数据分离

### Redis 内存估算 ###
![内存模型](http://wadeling.blogspot.com/2016/07/redis.html){:target="_blank"}
### Redis 主要解决的问题 ###
性能
a) 执行耗时特别久，且结果不频繁变动的SQL结果集缓存。(不经常变动、定时过期  广告物料信息，用户登录信息，定向投放条件)
并发
b) 高并发情况下，所有的请求直接访问数据库，数据库会出现连接异常。(广告位判定是否存在)
### Redis 优化(Codis&RedisCluster;多端口实例；参数调优) ###
### Redis 注意事项(缓存和数据库双写一致性问题；缓存的并发竞争问题；过期策略触发某个动作；内存淘汰机制；应对缓存击穿和缓存雪崩) ###