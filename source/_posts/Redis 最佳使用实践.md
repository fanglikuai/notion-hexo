---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QPNXBEI%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCICHC%2FYuToBfynsPsgaKtoCo39%2FVf1erW2nPdrqO1Phg3AiEA59Y4EeOIRe4t5ZL8X0J5Jw6YHRV%2BoBTvVsh5eeA8lasq%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDBrVjxcvsstuaRZJTSrcAwBh3U0WDXX0LMinLy8XLh2s88vaChChYc%2Fefs6fqsPPQWT9gVFhodkXJ78UpVYYjEUJaUzqwhRqfYP6Jv3Q%2BqyzoLHwXt4%2BOOl8ytvdOX1%2Fpzo0OkOttrwVBb93petC3nogKEmb4BX5%2B16GJqHYdqKBfqlXRap56LUT7bHizku5lXnkm65FpNns7nEp0QPnLLtMH5eokhi0W81NYyL62A9Seg5iksHp0oFtqhBRxoa8%2FrEuRZKk%2BT61nqhWyHQZbaSLC1gWViUGTXh%2BTNyoQD2FEF6%2BpV9MNQoZCuD40jm5Vvx3YObBnHSesqMXp7GRg%2FBgADcv%2Bzb9N65SbCLYbDl%2BBicdkuU7BLiapjR3qjNAUq46a%2Bd4LUdmpcUe2RUNv%2BtA1hqk%2BCXS9ufBlTGsNA2ptOZ5dOfTsrxx2unk6Gc29ZjYr5uHC8mKOwdgXGu2iGkgzfJVxg61cnwTb8CxGygGEkmkqATUSJinGkTSmX2%2BiK7nfz%2BG%2FvH55tOEZ6JG6GaoZAwlFNfYzr2Ri00Evk%2FROhE2GUhABpiGnyBBG%2F7f0AaWFjClsB%2BzbY9oWnU67EWsTWldL7EyPzha6XLD727U0XLCnTuBvuLqgZhebgE6uJ0e%2BRTwviTZf6FsMMHrjckGOqUBZ1BZOizvOcIRVfmNGswE2wgL6XbA13Wk%2B%2Bvvo4MI0tDXsBHDS%2FojrYNZwgExmOqEhQI4Ub6236TYA0M%2FiLOoiHF8mDc3A5INm5f7kD3fe5zP%2BoJ2i49qnbybi6onf1g4jbtvSUXjAtaXTAs4ZehCvgu8SWewNbM6J%2B4iFSrzwKjR44lkKJbM05BwoEL3GLb5%2BmXvHciNyNL6xZ9R%2BtQCep5pfh5h&X-Amz-Signature=7161b66ed6894e7f2704abeaf6a3204029399c00992ddd0917f2b7fac47fa046&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

