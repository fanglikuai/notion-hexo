---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JOKITTF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHSJMGCcfvO%2FvS6wOmGza%2BZX2dQwD7pHTTraCho2WBLtAiEAxstcx6s8LHRqF5oITWrneLkxvV4iZKeLDJh%2BRM61WGkq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDBZppnuKVwwuFzttMSrcA3rOaJqks7MTKFuiZHgUP3e3iURI%2Bl1RZ14z2YSYNnRSESOph4aJZ1vrVKKyoiDgS4A2UROsldW9F5bqJAcJLEfZ7%2FDsqIXLvO%2FZgHIv%2F8mEtZ9yF1lt9yK64oMlHKW18t6lQbmXJRuA%2FV09UdHWKOQDmBH%2F7mQEDSDAd8ISuJrF48XzAZ5xjjgx4v62exqV45t3IrBj28ZOFKPbvCTq%2BF4rtclWPLPlzgMQdwA04k666u7l4IQw8zWQeVKBg37ry3Jr1UWRdLkvPPLbsRBFenzQpkacIxsCuHjt5D9T8IB47veTepfoy%2BVfmVD4tlN8SOBVo8zG4Dc1Mvyy99nEpLdLonT7TbveV1H%2FtRxoBARkA8Gsdxn8z1lajzUS%2FDYN2bPDy5RXstpuoeAvMPfQzM0XTAn1BlcsG%2BnNGZKlSD8FNN8eMjVZn9KN3mgKhU%2FdrYB5KRYusDzw5wdlNgmtGS2mIAUxLK1%2FlJocl014ArTO1rG8LyXvZQ0GowS1V7a%2F7%2BJDdlt8SWfgZeKDCHNynKkQgmXXg8aylJwXDp5UEKamlxakPbdjh9f2MaSOhEH9xvqp%2Btd0mbZLzCM1eKy8OaGRtE9x4U%2BF93gFffoiWJK1oGU9a%2FH%2Fh%2Fvcr1CsML3R2cgGOqUBTUR72DN0dx4PfgELwMeVh%2FjQ%2BhjcnSE61BeU3Fvhn9O01HZ12nKD7Dg1E5TDv0Xf3Z1gVYajsbOWvvWcUaWUn7yWco57UsCOy%2BAA95zXK%2FpoAq4qPqGW7ibEOwYS1%2FP%2Fl9Y%2B8lomI2aU5PhbClV%2B3d%2FC99fj56lSDuEU6XGyjGWDUH1F9cT2xTcyLHsdA0HrwN4QeNhPRAecTqNAQATAp%2FYdEcV0&X-Amz-Signature=863d5118808bbf50ed273ff3420d995040b3e63968795fa833111ad235e629ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

