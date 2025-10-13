---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PS6HS4N%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDi6%2F0r7VjX5%2FXGFCO7JrSaEBtV05Nrg7rxfj0bxRMUTAIgUkRiZztXuOJsbF8olXiZxZk2odjzg7aLx4tpR9j%2BoeEq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDBxQ2TfBfx6Ay4Vx6ircAyKZqUiOvr42Vm7EfJLSpOh1cxe8oHKkInefoOcX3B6mNGM1GuvMuAySkhTucpaeQogJfwlSLvesfJ7SM%2BG9cwae5huS6pWyZb29%2BSkHr%2FYAaDlhJW0KYFjgB%2Br02RnCc9Dl%2Fma2KAgQ5b9Et7cN2vy9cEqRjt6XStlHE7pmMgOhjkVIlHt44isP7HNoCLUkRnmGWzuHmlQ4c%2B17JyrrK1Gga6vmyYixxR0SdV4Xa2RY74CFOMioU7Xe4okQ6ll%2F%2B%2Ffg%2FS87NGiOE%2BAmNOiagzwybinxt9OS10FwDmXvGL7QbUyjQC66ufeCDXrWVu%2FYzH83h8eKjjmTCUxbg%2BTF3s8CnHYAl3WXFKfq8LI1duaPczjvygoGReXXtnNk86vuqqRE03mrY4bdlvyOF%2B6j5AHHvP3A1IQrEG4sZXbw2K7guGoppENSP4%2B5j0Izl0F%2FbkJYsS62ki0DWWJOgzyafzuJWrhqL4crO6ONfxv73QwB2z%2FgFKr6F2lTgRS91ff75NLH48pLCYSAgOiRAs5br6kAe1BTV4XCbKt4d2G7ca%2FPtmB%2FFIL%2BljZvv5zXx4TLolKCJ%2FeykMF7mCJ0CK8GrGDxtXrZEFe%2B8PgxSI3WjKvLsdyokavHLNT2kwjSMPzOs8cGOqUBCzYqQ%2BrdSljzamLvDuAN7YB1qpFETgKZWx09NQDenSihB7Cup4P8y5pWImozoWV22D5LNmzL1lkNt5f8QbNqdHN0Pt79863ZI61OLCQnb8xvljLnalvbb8D7NrEMIn1P5aoOEPstR6iAbflt5lcgyOzB%2BY7ES4401jSUAJ40gW2B0X2P%2FAZuYiOzb9bTj3K2O87QVrBe80KTTzZrOZq2DtRdW7KZ&X-Amz-Signature=339808fd4a95297735eba1b1ee1f25f935c07523b7db180b87ff4ebc50276a03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

