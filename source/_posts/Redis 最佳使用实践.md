---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFQZEKPM%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIGjm8Wa7I5ek%2BmxoCK1cmxF1HHH%2FRRTFihnRqf4eNkWwAiEApm5eNzUCLqn5YLDQcuGuwCLrwPqZ5OHGSyB6z7OzqfQq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDEv0pPasOzjH1w4qHSrcA6rRfjTKRheifnBSdgUW2XNRAT8uKBVpy%2Bz2GQVCx66%2FBfilSFzFBd0gXiFL3ERlS03PzWelq0mCtiTMXDikk4PKMLch1vjeR%2F%2FrGbBUgqPfGCXsbWf2wiE0oHG9OmWx%2BwmCDnSfnAudjOJL%2BtItLBn3Qrin1piVvXQnQK7bHck0UyzAC5wbduqutgrieUGUu6Quve5CRJHHwsPPbFbTSUsEsYsF2u%2BeCp%2FJ0BHpjuL%2Bz0ZTo%2B1EfJc9hlI%2FNmTc%2FyCZq6qE7GZ8Sl9%2BJjEoPj1XLYfu3eLrs1Vtu9typjNJTsyWPc6J4i5Yyh4pAmZZKb8A3%2BJ9mFgUCKSw0BBurAg%2Fgp6jOQFFp3GA1wnnv%2FNPVdJibS%2FWxg4NOr1sGz7KG%2BIv9Ezeg7b%2FmGlDxAV5ucvCbT6TH0J7yXD2yW%2Fb7M8%2BGSKVcYzfCQ9Ro1obAjMIClopUWpw21qamp%2FYbSiF1r5TCOCBVcpptjcJjkJapu4w7BVHsa9MyNOMyxFy1UZxGOXgjNslJqTeN1CX6TU42Sk09PEzYiGrqVTanECxNEobtLiTSFCHMMsSf0%2Bct9V7jS6pilndv0q1Vhkq2ppWY%2FcHRdYpOG79u%2FgACzvYpS23S0WLtQF9IHfHzcIzMN%2Ftg8kGOqUB9gDxE%2BTkB3m1XJGQTIMd%2FBkvmj%2FTYK2JPC7tUCxbS8ZR1%2BXiJqahKwFxffB4uYUs3cz2y1yELVzkr4lg5Jc4JHBZHian7%2BMGY6RixdHJO51B4xSWpAu2ZO5o%2BopTthPCEugc3JbGY4bhOHaLTcAp%2B1X6Wtvm1IvPzZL2%2FyMGpm6zQdnlL7Cz5cxYlvO2%2Fp685S%2FlDC13HglDcQddONDj%2FMJsc53f&X-Amz-Signature=944636a115d829c1b5966e9d731b8a4e40a5ea08dfb9a9a72553db40cb86a3ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

