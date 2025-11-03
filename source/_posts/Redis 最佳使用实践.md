---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPM6LT76%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDT%2BJktxrsBmwsF1kc5ysbNO5S0wtDTg6bWUHZDYu1vlAiEA0vyWA9jrkEATmA9dt1oiH74PdQGu8Pfofyk%2BWghe1GEq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDIarGWX1WBvkr2QCJircA7dpfIE9UDDCpYbvbVOyeeEB8%2BKr3LLDjUWkbvfS2kP6drS%2BDYPMgz7IIQmx1rrqVFW7LXdPniPvy8XipkbMdNwiIBSB4cqLhBNsy%2FqvNDyLEhvFCvUnLZMHwXYVtwgTOU8maUajg1zWkq7zMUbgBS7mDpijYNkpJv8VD3fAE4kVkNWPWiZOnxvyMMywBGkmD3F7yC7S2xYOf3NIYW7IlR6xpu1Ad1bfWP3A0tAE2qTqizM2O84%2FP1oWAiPbQgW0uZpLz19HClG47nXsKAtmTXILCOM52bJ2mAoE%2FE5mLY37k8PD%2BlpPOWC%2BEnLnG9x2VzOKysjNcz078mXGZw1naXqu0rOyrODF6OFeYP3ACAjdcb%2BKMx54NGNtw1wKzkOaGQ0IrthzPyseHmIf8%2FLFqQoMaYv9jDlhC6P0w1J701rLkQFt6Sq4a7NCr%2FI3ZlMR8dEpXUuf%2FQ%2BuhhqjElGQPwVuwZx6LKoz5xQRmti8eLBs3RRzeFuyJj5V%2BxetuYznopU%2FQwiDJ3Gac2ihZ0Eg5yMdi80Onlu0Sw3vzUEcVRuFc0ywT9dlgPTDtaTOzoqmaHAQhn8ZSz83ENricBM%2BbZ%2BqBL13QxG5dRbzZcwUpa439%2FG3jP9D5TrODsENMODQocgGOqUBqfFZ%2FAaGBN46fvnT8xT9zpsxOehGE9qvJEKcWgz06SHMg1BkNWvGGj8JNT8g0t0GhWAfo3B1dswI5jxAibmFkiXuCkcGxl6i0%2BhQsNxWlhuJh0Fq5NVxi3gxC3ud5gTv7bTNgJ9eDFpD0K1G0cBtyqlw2hnDixkQsin2EtGVqDSjEyJB8kftHOI2AG3LHkBt2AgxVUJmOEgpEvkV8IW1JiX%2F3cIa&X-Amz-Signature=d4f0e6b91e183a4f3ea3484bcb1131583b295a4c5a6c53b37b6f0e7255497fea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

