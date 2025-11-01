---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVUGGHHR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDVFfJxfuU5UpD8OuGoDlT7rhlgXRpdY0j%2FAaxkFRq%2B%2FAIhAJtAj3p87NTKXbjA4HV3CAH1g72WVICq00h4TY%2F1AlytKv8DCDUQABoMNjM3NDIzMTgzODA1IgyNWCP4oh%2BrNgozHhcq3APt0I0NePVaJn1Nnij2X%2FttqbhrmQK9ltepd0kPc8u5Gh8LgwAKCPoq31%2BqjVGLgiOasIvCUCeEMcmdQ077P453D2DtczR9S6KjhpnBO0dbJKxKi9XGvyBbIPgGXkmQlLirhvW8di6lCZTDo4QGGrVOxZOAu5YQ0gVRrGG3IrlEEhYvOUY5TZSKbFEhoXGjYHuY7kTM3r%2FquGc0Wh7%2BxPUC3fytCcFjV1%2F0%2BosLBADaTCoiqyXiLjkPin75qGu9HNmlS8TKZvi0dMIP3AGSZBjBWgf93cxyC25lgyaOFWfbupXBDoPW0NF18iP2h8%2FvAUmxLA35%2Bb0LP4r2nq%2F%2BNEZofkSvsvVRd0UY%2FjmoDwCAP9zXu5r8GJIplZ%2F6%2B2mZOYOWyRj5KMjUeeRbZ1CF8pKkGLMy3j6bgrkm5HJVnaRTCvIfT1Bs%2BiMz%2F%2BL6JBhORnyCTTickAifNXSLLrviAfQ4eUMZbh6ScPgjYhMp3t3aJJ871G5Ajae%2FPJnhOSDeDfdmROyaYznYFN1KY5Dbx9NZ0bnHHlQ12oOr9jS5ngjFkbeCBUDBhHiM3Uku%2BVAPH3KHrzWo6dc9NsekLrSpjYUktTpx9W8fE9BVymLXrWbwMdSCACECQ6WiBfFE7jCYw5nIBjqkAQYHweVxycOJ1%2B%2BLLPAkB3AeMMR%2B6jOigy%2BkV9R1Nzu8SpcIWY9r3Xx2CLLrwxqd8PnPI7G0ze%2BVd8rX1cL3s5UZJgcHutzF4YvFBxUIzkeAg%2BxqH8mt0RoWQiYjrOqTCK3Pz94n4HFLig55TwuIGFfOAL2S68%2BGLYklBgtqPDBXbCQGvqj2VjO%2Fr8RukFKteb08yMNOnNcN7e%2BWmKJLlEuips9B&X-Amz-Signature=9433cb2f733d34c13fede6323572e135185be49d7e36b738b036e299134ef423&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

