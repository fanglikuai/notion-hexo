---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663M2SFTUR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCyy%2Fak7GVdkZrPBs%2FkNis46aY42M2MsUeip9n0ECOneQIhAIvKzKKLDNeEx8SMnFlPfxe%2FspKntt7eAFuxUkiVUMSaKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyZRDg9Bzd52g9xMAwq3AMH98JWxsX4nuo45LiLxo7FCgCCChl6GqBeoMKPUK%2F5tkJYJWn%2BbT0v3bSbBHXarEp0I2Zk7BEFWhlQ5DMGrlEmOBzeBUtdS6OE5sArdXuJIbXrnqoFfBCgqrVMYyth2MAf4bVCTWc1QMN8t1JaESDZQts3etGd5M%2BvAEwkzzMnTqBk0kt6UAnlnCZBa%2FREjeJ3ZvOJrAgh60etcAa%2FAktjNqapQ3j4c5HQtwcO1IOC5cLIVJ%2Bq9fTZAM1PP7h1TIkdT6n5F6SrzMmjNybDZ4hiSNxBS4F4ZvsCBWcFu3z5N%2BHFB1fR1tPrCWlXjwPSicc1FLBoH0vP2p8FbnNkcH8tSjr3U7r7V3xE6cE%2FVLHM3iwL3DgafI3uIVlYOi8WCJX5iESnepe2cewlJj1tMhPV%2BdOenJHijHHXZCED5JkuEItyx8VhXm6QpWTgEqynPGNQOZ2Gu2tH2ikb2jjs%2BxvnUVqeqX5e6GxtnWTH7Rn0MaTpOn9zcsU2KKVqHrokBBc65n6rk3AoBWyN4vQ0l4ijOoV5eZda6x473OiY5nb5oUFxKAS8W2FvCXRch7sIRBT0KWz0X8iuj5g8h%2F%2B%2BAfYIrU3qTjKrlu4ECTE%2Fo9QBc3ERng1RkuKYetICVDC%2Bh4jIBjqkAa38q%2BiPuqlimf6IFPnWt3BICXOb7MaN6e7M27Vsp0bxC08jHv3caNNZPi9SWwdCLKe2nHogXHPZdtsPuWf7AOYsPmVkMtPTFL%2Fep8Ue2p5Z4cV%2F4i%2F39DpOwlW%2B1NRdMYK6dQCYv0o8y3lgK75OsO19j%2BKLJyICs6O3aXS%2Fj0lXT7EHp54yQKFyzCtkUr4FBi4Q0WD4bVP7DCLqIQRIgdvjjEP%2F&X-Amz-Signature=28caf837ec86d20e5e3c67b53fa51424636f6d16bfe1b57397a905d40b691057&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

