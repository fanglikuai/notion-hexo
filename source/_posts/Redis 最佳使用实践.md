---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U2CP6DS%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUcRib5yN62JiHvtLZc21q4gAAgAVXpafUh2bncCuMeAiB3JYXJ5hZdsLap8zWTZFRB%2FJGp%2Fbfs6HU%2BwzdDg2%2Fpbir%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMKCF5NwxauAUDNjLPKtwDm3gDQAqXvjjYBXWNJ6tpAej6P6r5KOeWt7L1rWMrmggs6T6aTXgTMAIK98oYQYIMdwbILS9w8G1sQlP3OBFsUPCzMzQiwmYDU3N2NcpN38IDQ3M7bWIscbQtfhw0VSUoxXAnMvo1wnMl%2BSgGJ%2BpkVoZ7ApL8IqbNYb0wO3YFUBb%2FkBGzqcoPihV0HjRZoMkbr9SFpFBVlpY3I6XGM%2BsnAN3eotVeJBzTD2vfb357575vTkm%2FrRLp4vFrcHxWJFLwgBl0wH1WqSpdub2VdQdjl%2BbrCWbL5iJIlBBMQMj8wtBAmklDaN3I9DnhMYPG%2BoLHS6GBY6ISF8nRbKJ2BJ99ElGP5fQ3gn8kJPyirebR3uzYKvejvVs88mm3wtSeh6YD6uFD7Oc5XUYEAAookVm6j0pNJVl8iK2HF5n0fyZKzK3mwHCFHELClLufw1wNn%2Ff4lRfYvyBR7fDacuer8Pjw1S8KcdRF9mt3HhUFz89QT9pmvRfMPpKM4n3xNc1wB3Z9zfXbGyMaRdwKlsBQvgqGJiiF8DsngDVRo61BwZAR7dK7Ll9ToswFsW067pf2C9x2DMbFW204QPNav%2Ba2JL%2BYqE16ZJgyMyhcca4kXI9J2qREgp7bQ7e5Od86M4MwnKi9xwY6pgGEh4dw36%2BHF%2B6jAXrIA%2FSvMSmJVNuJfy9xEQrhriGgdQ5Eqk%2FP%2F3Vy1eK%2FfZ9%2Fe97XXDdELI31MPJQCsT%2FYGz4S9guP4F%2FmukEdPkJwTjAKPY1pvluc%2Fq3e8Ae5Uib8BjaMXYzph8SUI3B4XdMdGjg3CUNtnMDxXtjX8dKqwqwIOfOLY%2BIzxZkvtk8TWwvlPb94H5Faiml6YD7CDmbz8MsKKoYq0ST&X-Amz-Signature=2d7819bca2322e5567b41a1ba0e60be2b5471a4e97eaa31b38def530bc27f195&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

