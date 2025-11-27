---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VXBMKJCB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNI8mN4ZIpa44eyl6kWRdfDRTqzBo%2BNZ4BJmoXXiAreQIhANMs4lkbzFcPela10p9WmE7oi%2FDDQq%2B4gNfOxuAg8jhZKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz07uZ0A7orINFx5NIq3AMUT%2Bf0dLRYASCnw4ebzzb%2B%2FTw9TnP9h6SJu5Evj4XhL2Xmnu0jyI0U61xutQuXnlIJnsjRE6JZDGu%2Ber4%2Bi0YIbT5eyyFD5cl9gik71qjGZLpOTuFeASjFdMFcWGYjSOkE7MvRZQkXWI1W1DJl2HtjJg1kr2U5UduHqh8ejzZOjbIr9wJSLBogRhmcck2TZh6Ps67qQzE6WI4%2Bo5Ppwdfupj2Migc%2F7ZtnB3XG84L3dOUZQ730BZqEyzbdfw7VRIsAnlMiR2E2j3cCwMq9UNRG3pxtHePlVIKRrQJB62bkoeVGmS8cuc26UncJdRjbS5fT7aUYxaB7F3j3bEs%2BFDSxE0WDBQADUMDJBF%2BO%2Bj5dNTdEiWjhNI9Fob6mVjoVnhZGjemX2XUJLmC%2Bwun%2FZI5ldsUr1Pz%2BBkCoNHSojePeA8p7A5oYA%2BxIMfZnPa5YL2%2FAwVFFIyEhXpvw9S88V1ZF%2FdPUGSWW0HopHkbApkvirNvA2bIlYwzIPvjdO4L016Eca1cIEX3UEpBfCbIumg20H0D%2F%2FVzaSpa9ifK5EL49OfZcQAyoDOQDvczLBXUuX2vizc3RXWmD%2BW198Y7uhF9BFWHnqt6G9QdsGsmFho5a5IL9Q0%2B%2BYdWo8S3kcTDSup7JBjqkAVEN9PKLpQGueeOwBNsvqD1uecTJNQheq8seTfXohUwN5D1NCEztC3fo9FGuK2rVg2efoZjLdAkGU%2FhpjKEUsLrTKnV5%2B0pudh0zgD5%2FLmymgu9CXDgG5yimvgXU0RfvC8mm7n1H39H%2Bk9k7qezg84%2F5%2B%2BcnxEOyoGfUwr83RuaJjjeOAKYXfkJx8BIYskI86YmHVIOjRCBKtrxyFyUKOuTzGc9y&X-Amz-Signature=777b125f471df6f0c1a4a21b7e1d30160886870e698c3a3fca8d6c156c38b48f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

