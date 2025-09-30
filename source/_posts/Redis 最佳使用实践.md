---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Z7NKKGY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIGBvRQTFT2e2nv2Aiefz8EBwLFNzsG8n9mTobK4msEq4AiEA7B4p8PrnETzR%2F%2Bq3K1dQKGpHTfz7ZtybCskWBcfd4DUqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKYJP8BSu%2F0e9uzXCrcA9iMdH%2FCFk7YL8pmVpD%2FudfIo%2Fe3gbT9bOI4yInPi1oJoiSAHg0RDCc95cuORd58qRs7NFtCnofqIfKofvRKvwcNfqmfcbdu6%2FTzi1DGDr5A76aVfD72AUiW4WO3K%2ByNXoroy6argxMVQt5CKj43R7%2BB4ww%2B8ThYEUTFMz%2BQlxKYtFmdsb1a%2FRrDUvhMZ%2FB8D5QbLEfTHBEkir53fX7I1LaiteduTR4ee5EHWp4Av1GodD7pC7BvKsxcH2ajJ414R%2F01BDpYmyOw%2Bh9uKfu%2FJZHFn%2BIQ7HvfbDDKuJLe5RHt7uv8g2zJZiA%2Bhw%2BBdygLLYTr5KZpzy4R0anZPglRmr0eKWutBwf%2BT7sc8leJHja4iR6nB%2B4BqO7akA5JEuauZ%2Bl72sZtaZ7wjAQSOjk5HzGSd0wjMeT0zx7ydubV9yzwXv7ylyl4AN%2B1pGS2nqKn7Q1DsdmUnzH7Mfj39M0FrYgrx8u9wLSVKfXpVrNkL%2FgsKB0RnH%2BheLhbyRF4XsE768zFixp6hTuIrbUynHFa1WaJCemr8NVGqiXDTPKQ382svAf1%2BOxl1drJ3cz5eieqPRz2W02FnIC0FCh64yKNoMZgIr%2FrjAHvFXBc5AU7B%2FIfpK3RGQUJ5MBRj0z1MMbY8MYGOqUBZjSeTDJImOJ6gHeXHChAntw2DCPwUL2%2BYq02GOkb7uYxp2uEcVJs042gRbeaZzMeZZlwnZk3XiVfvCDaMv11GVzTpB8mob9aLxwVCi7omeLMf8y7WGInYaGtN5KyJ9u1mm3vczROJBPXfgFfH4F5U6loq1dUohZte1BCs%2BxWPH80c3d2W%2BOIEAQRgCKtSvgIDDRVbmrPkKNV4xVxCPByo6J1t%2BRR&X-Amz-Signature=8c03cc207ccbd29a6ceebc52d664a93a50e47d7455d556067eee3f3a4e0d0aff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

