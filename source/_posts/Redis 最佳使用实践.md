---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQDSIY7H%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQDa66qMcwSlMWxzoFb%2FZ0elNhlTsRtzZCDZMtfr8k0TgwIhAPwjwCNQsTqqI3g3pkcerbdyPUd03m0nfsokC6orIRFpKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDjIweP4yynyv22g4q3AODz%2Bel0We08otVcX9LTy%2FOwcoPY5E30TXjaT2uJKxxHC%2FdeeiAUoR%2FbwDAVqSJFouvdvlxpG7Q9XcVT7jf2pPoouDscbvyg99kDw8%2FilAssz%2F58QuCXmqalIFY8UMuGq%2BTSKMBtOfmSTYC4ZtvKZBIo181EfnvTqPxGseg6NmCeac9civdgUbHb0IDrEZRMskfqul0h7JODp5ftM4Wr8OaeqJGuvdmqG2axWBvDx0zmJm1MX0apuauvnIS8AiA14cemeEl2Wm%2FcfYSLb66A40Ze%2F1Yl3u78O6pVHOEUQxiktIhfBz7meaW4gA01p8qjgG6jgPmRg4GLPkFAg8cwsR6ej0KTgQvf5U5h0Ig%2FBluDgdmdRvStd6LlOBpz4M6%2BXSL2lPI2YdW7ZVEGpTKTRdbRqsZne1VJPCFqJ9fpK1uCHjHwBMBK8Ig%2B%2B4eieL3aaTMdQBgFl1iQlpNGxw9ofNT8p30xP8bf0EWjLL4XWArCoHWM0tNxNsWtu2L0SvL%2BkZIp2Hb9EgyJ%2B%2FMNxb7G%2F%2BA85KdGCG7ilmf2mHs2z0nZMPVyi6S%2FdmwHyGQhY8mykcFL7v3EV6EWWDWhGLdJl%2F4aFcTRRjxso7XF0I0IUwwhkeFyZi8JL3ZlqvNgzCiw%2FHGBjqkARBKrdZHNjg983SnaELnHpASpBS%2BL2RnHVUJeTX5ovSHwGr8Bkq%2FjLi3XJzg8pTGpdHRDTUFKM6SbtFCjWacmBvKNQC%2BqO1G9Mfi3iRiKa7c6MpSmTxBNfjbXfjqOKyXPGRJdINlwr8RuNEKqx9FibJFOHsdUEJSJYo6%2B5BSb1vy8rQi9X4bRb%2Fg17axm8GrjvOt0FNT%2BQ%2BLZoX%2FwukfAMN8tN%2Fi&X-Amz-Signature=af4fadc67f34589942f28181954c0b4998277290ecba974b05bf447377b6afcc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

