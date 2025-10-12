---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466754E2TLU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIAUGyC3HGLF8MhAcP2TxzL3J8ongq1U%2FkP%2BCYJv0wRaCAiEA3bf59nfsuqM2fx%2FHSVPndULXf2qiSUJfeaCuENfSc0Aq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDL2FgBilS7tHOzbBPSrcA1sogG6WTrdk0I462B9%2F5JebI%2F4ga1NKo2kMSbIxegBkU1bQNL4kufXLdTvo8BAM8myzRIkvzB5tGprhaPhg%2FL1NmEybbvK6lE02Lmb4TaKzagPGZi41mZdD%2FlTOhBewZ2fO8dPPaNHTwrgli%2BNWK38pa01YjeXvCVyQdwz9hTN%2BO8N1tA7CoZZJCVzJtfqBX%2FmH54JyHVn6FSqfe31gSi6Mq3q19W6OPzQV7KbERGijctm9gpDAM0Tn%2BIfNJO%2FoRuwaBQBRqaxk%2Braig%2F3SespIWnGDDH7m0Co884iHpZUUzJv3gXI4YpZZVgj5cDJGJWhj%2F26NTcKtvb%2Fh6IdkuCoYN05DJrloxurykQ65VNZ5LVxTPJ%2FyYKnQxNjaI9%2BH0cdus6aRvYzAHgki6d0gFOP7Z99Fxgr2pBhA6CV2Cn9hZ7WZDamf2l8A9OB7RFnT8ypQ3259wcn66wU6dZix%2Bu4eFD083SXfayRzAAhkKVqchHWd7sS3e2Wz5axUJVvT26I%2Fg9UGY8FCKiswcMtEx%2Fl0AswAhsdEd4vwhQyUiR8gaBAPNabzxLXXDxAZwQF9GOE2fk%2Byime9AVnrMW0pVCL3AhNdDvX1W9aNT6qs%2BLd5gWiEXV7Kf2fgutU9MPzFrMcGOqUB2J1cW4No%2Fx7u%2B%2FoDay0%2FBaCycSBiY1rLR6tIeO6OloMcCKziY70hvkZlCXIQYmCBzmxn5oIqX7mAu%2FiQzL0UQUyi4p5f6i9YFtqLRdsk6nXkBWuNEFMWj2Slv6gd57zhu6kN2MVJFi8kvmsqFU3X2%2BNdiSwmHV%2FdccdlV6iLvcbFDQmmemAyJzHqLmqw4WnekonPL6gOHYnW6oVoxZFUzpMU4%2Bh1&X-Amz-Signature=23bead73f256306f1ade93dfbc3b34dd65d74956c7db580506c28074d3b4ce23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

