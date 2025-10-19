---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLFUCF7D%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIArjIJBXgNl6IsUmBHkLaEuZJquVuhvF9uIWiYkldi5jAiEAvn4Ecpbg%2F99YWLruOOHr0%2BCeHhpMdzksRpA4U47ALWIqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWn2e%2FDk6UIwZXhWircA%2FjB4bH3GgzwClnGb%2Ff8J3LyDL04n%2BB%2FqebR5YN5NEhQlDD4Wscb9Fvy%2Ft9LuKjpmK9PPtoGgEwThhgBp2JGM0IuJoM7JSrcBXsDqsqWKkAPGoi%2F33beKJGRPQPL2Kz7eRpCXukB9hH3lfbH0ZxTmJqL1CrJOwbAwXJLkdndSNPWr6asd8a0RK2jOrctus2Nd1%2BaAeiwTh33v2OBLPNnDtMBby7r2uxNHpI5Jv9dLMJfOSiPcTBjN9Pvv2h0twqdvWCJ9mRXzENUm64ExfAULyDUYl0hOvrZ%2BLsnNBmfn2ZR2f0EcDfqZvpDrYUFuR5vnGHo3II50%2BqBCzi3551ty3e07ex0SucKdA2YOAckxPftFQSoABBzbSQaslEx9MFcjVbawYtWVuLq39FGVG2gsd2cb9yvlneFIx1ESrURSwmsLIBFtxSUD5JF04oO3rduk0G0jWlnP2Wv5Ow5jn9xaVjjrp1oIhcCNEhidSDgZCLjCz0WkQ6WlZIKpnTehPk31u8ED6Hn73%2Fp%2B61N%2FRJyk3IWCI7uDw8HkwfwvBRqdhw0M74I6aCVDHMooSJoTh4pGrvoT5JEPib57tLzRHoGqo0XjmAsAEBisDlGNpn27QWOzEmuMuFhE%2F5CBokOMIHX1McGOqUBs8s3OPXQ8e3nDmhcOcOD3vdr6xJ6Ch%2F3AwqSuiKvMNNlb6dp7VhqzOc8SyRsdXa7exR5gHQrFRQy2HSCONVnbn2XosiUOPJD9v6JH4IsQ3KZYE90%2B6e7aTuPuX8rkZUBscC%2FzbuS7OYodByLexc1XogDCNWZfBywBmEqAPBTxm4WxQOCBvKKeoAJITV6IfVq7DIUSuviGgIlHx9TCMVZzMVNxxfQ&X-Amz-Signature=83c04da820b3f1a6d1b70c642ba4c7809944e6a77d80d0ce13182a9a6e237627&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

