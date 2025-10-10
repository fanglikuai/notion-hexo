---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRQQIWW5%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDwPCedgZSa6LDQyZbdPZ%2FZlnPxaUpspkx%2FIEi2CDwDwAiEA053CziBvsxupxkkWvviwa4dcsqnoFf0EyrUysLT5hCYqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPew0q60n3QCZFSXIyrcA9ljNMIVlgxb9NeC2z3uSfmMYRmAKYiabef94kQZDVDGHL2fMwItLq5%2FzWo%2FYf4%2BudFee9ABNA%2FA8So4LDlsm2ki%2B4PGQ%2F8B8W%2BnmvG2leyHhqUmEdDYqE5Ckj8mX2cghGGj4CGhmmw6vKHJIeoHYzHoH7Mea0sp0pckEqzUNuvyctBfAFqEg4K14JdygHMCJ9q9wwfKf%2BGeSQaiRKRrqniSzBQuh2OCWk5vtPpfWXih1lhMChoWj25GNiQc%2B3dIglF%2FlfpDK89sl2vxgHqxt4LCcylpHoEjs4%2F8odxR3RfKNMvVNl%2BZUmkJnKp4BR4Sk6z7OLhlkl%2BGzHhxU7NwKwXcuesAwxrUQU9t3oIe%2FThHzaGwFhs90TV66TNYjlb%2FC62E3O5pDY5AatpO5mZHo5AzZJlGuHBjVCTw%2FuKZ7fBTqocLpx1SFPLDn7pnxb%2F5saQWfIV%2FzOfqBvNlTjkH1yBC77yk5dTAd4w7tdaNRz78%2FIHlQyu7vgMYLfEKljNfSJGCRShCzwEfD8yGWXz1IDJd5L%2B4mlm79u7fXkbMKT0bUWBByqKz4SIPtD3DyuftcMkdLl%2FHorLCY4YRCLUPQW6GNry8tSOkzWGoZCViHKntObMzB%2Fy60oCZyFNiMMr%2BoscGOqUBUVwWj7%2FsXo1CkVdrp4gJ5fEWbJ1EOdP34bMjNXw%2FNnZ%2FH%2FkZROg55tX7Kx1FdBY1m9zp%2B%2FSHIX45XffjuOiBVpR21whtW5xYWsnLauPqytNWPUhg%2Fx%2F3quFh9GXlAmNgnDagXMSMIDXnKAS5Phd77rIuBMzgxAiYkbj2QdWoxIl2gOFSWRjfjF1O6NZXVB5ENKFFseddEVWYCIuRhNr9TSMD%2Bz%2F0&X-Amz-Signature=f47656f2403e8e7aaf1c185b2cd022e585d5b8da2dac9a887b355fb5afaa1dfd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

