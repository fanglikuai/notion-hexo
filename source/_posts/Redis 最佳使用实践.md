---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HUIN2M%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T050040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvZGmo4Bv5M1JqIcl0qmFkv1dfRH4NzaVRxkQyCWgDJAIgJuEdMuWc4iRx0PYVS22CP8Oh%2BtcIvVOtfHVn4sID66gq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDBnaLklzKPnEMs4kkSrcA5%2FcIEi9ONE%2FAEvqYAdg%2BUH9oF05g4lEVM7Mz%2Brj1f8A4VErsjHwm8ZcSCdr11GU1cTbAWxhcgzBD6M2Bi0vauU2AWB2R7HhnAuikEEJ6vNr2U9kTZeI6XUCuKcfMHsbn7IFCnW%2BbIEr%2FkJax2JgVarLS0SG0Gl6JziyS8SQsrSBBSZo%2B1ZnSn2hYAPawXTgW0TADnRSV07liwYHlsmpMfemr%2BWoxhbhP67uVjcYbQkVPIPjfyfSZg%2FOevp0xNM3yxb%2FWc1p4i4V9%2F2oz25TfGy2VK8%2Br8DHzGlh0ZjTysa2xgvSEL%2FR42lM7WGhNusnrhiCc2puF9HnV9wBxvN4RGYIyAzs%2F2e912V2WVTXSGgj2Sov%2Byi68117Lm6IpM9TJdfkRl8Ka4uqMO%2Fcx1pTFQkcA%2BC4jqc4jf1HW3N9yt1BRXOt15OiVYV8Fb%2F0M7S7CJStnzrtb3G7PR3yBnIfKSoAcDozJ6uSuyN6kT8EMWU0bp0t1Cgl6WhVaL%2BPJLFFUY45zj9clo0M5Se3y2eG8xhkyOPMrhrWP4qU6wAXATTfeP%2BxmZL344fIs5P0nWYGgdJtnMO7kA%2F%2Fs7JBy1fkl5D7JXx7g9Eeg9W08Uyf5boduzdLKYoSqLWB4I6sMI%2Bct8cGOqUB3WUgSN5pp9RiTfXH5yMbhlN%2BiZM3buvbq%2FJ%2B9YEmU8cMnnsgbMSiTdX6PPnY%2FZIV9hQXEDXEvkwpdDJr%2BIGAhokaMCjdNRo1oHQh2QseqyUR2nyIb%2FHXw53XVK9%2Br3pze%2BacBCvq7c6xZvrXmu%2Bq9MjzdffdW9A%2FuI2vYEeiq4k4rxe8slLaHsMCtCNZK42Qq85F07A%2BZJWBn62%2B2V%2FPZqsCfPXo&X-Amz-Signature=3f6212dafc45127c0924b7e0a07cdf7c96f239df0be68e0a8d7fc033549d6bd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

