---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VV2PGU5L%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQCAz%2B9bMq2XHkeF8a8caR58c9hpPbK2zP0nBZEk2zmrkwIhAIZiblhevLO8%2FdGqZX3QntpwUsvJXk1DVCTv3NKKsHlyKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwJxeRhLCmVdJ9g984q3AO8cBYogSC436p2Tx4c24IYJOOYtg4x6GNFjlLoI4C8TQt8R08n8Y%2FFfayZ1NzxQCOSzDzA3gX162dnzgFzZncgcO4Plz5tihKfmZTHdGWiwHbJ0fRWQ19EVuSay94SOQVbVDAShzVx%2B%2BXOT7P%2F4%2BqlvdTkJIkjoNKDiP8bYwEZTZLba2jCbIb2U4LyEOboFvTPWdDZb8pfG7PU8sGwbFccOk0Wa2LCI9dJpuiFNxW8zKcyzatUNUPFq02GNjWeXP6DMaSBXUeb8HD5mIyAboKP0RoC%2BZ6hL4iwcqqxzs8ojpilhX0xB2DqYlwSnSuftHD%2BGbGo2YHZTQAeKC9cbppsZ7pwI5iTGTHHEnPQOutVDBLF3hxljF1cz7Nu8Lud3W3Xn2FUo%2BDTiMlR5s1iKpdF3uGleKfcqQRj3ACVNP15nuvjIiyf5tAFOuvNyiDGJFaRyYpgc8HKKuwSbC2Whqz11%2Fysp7LKsTcdX4b9LgtEEQUtfyWKdelDAWYlpaXJMt3wWAXJKgRUN7e7m5EZ0BbPYrwuHcPfSfb1AVr6H35McURoMHTgowU0XUT2bwDRMj2xc7vsPs1vZtOK4Lh0prJqA7gMQRUX1PDik6Xari9WOnuuc6JVe8yRD0hk7TDsg9vGBjqkAfK%2BmH1%2FMyGQ1qGp4uVZIH1xb%2F3%2BRGaLcKXg%2B4PKU9HdU3dm1%2BqTKwqCMHoe0fTC5KA9%2BaOpfPz9WbmXZUsRSHyu%2Ft6n1FPYynuvHnsVsJ4FqvdyVdl0x4NPdWCJgR%2FbC1HY49UdURCiOQHwoY1F3eOGlsf9WWQCoEHVHRLbsheY4FOtoG%2FBGZahNDlRd5hWa%2BBooAYr9mD1cUwONnxjMFzOaH4w&X-Amz-Signature=2186ae931691b8d24ccba41619755a67ac9f882c18f25fde0bbaadbac213cf99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

