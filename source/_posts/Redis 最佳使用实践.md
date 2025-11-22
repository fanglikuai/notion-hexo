---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKMMSS3P%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQCpNmgI13ZSb0c6ymxng%2F53cKWj9CgUuhvrIbFzvHtqXwIgY36DZLOEHD3ExO6CJ8HvOANbg4d6ySa%2FbIVcIrdRn1sq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA%2BL0Wdyb9WT%2BNUAIyrcA%2FtP%2FqcvgVVmnmaMLymjgD0yRcz2z98V3uEx2HSFXXi6obd6HoKR4fgV1nlC4yeYqilKEy6Ruj0JCGcReEUI8CAqSA7EmxIVppGLvESmZ5W9Gcm76sT5JAHFcQMuTYH301LBXAsTUPMxVem3ek5XXttm3psO8ghbHfJFiZ0woEYYYYS6ZaL51lS5YDEh4QtU8R7FDrG2hTx919w%2FICIQ%2FgX4qpPhova8UgoDtR2sk3EchUA%2BfXHmWr%2FOm%2FQrQZQTY7EsW5aPbcXARQz5XjPOooJNMpTkXEc9cZ76eIN%2FevKhV%2B7X1xpaV9QJoh51GSkirpfb9dpkUu3BuxMAxmT6g2%2FFuo1kvnbQioYiVz7kAOC845oGjZ8T%2BivlPEyj8%2FAECR6mlgFZ0kDKgbmCPvp0gn%2FnO7w0Vi6JpvoUYaXj6Ntge1tQWeffWA3JeflVrnXWDMVZkRxrc85O9Vayop4slpFZt%2Bh0D2LcAuXzSrW3rLlPw00BogBLcl4LUBGuovtY57BG7pqm5j9MKzSSSnIk57EyM12xQ%2BnBXZe5yET4PEMvkuD0z8%2BddN83%2FUTrYPFOQ%2FQ%2BjeEb2NpYyqRh%2F12xQlcNNFf1EJUdk6FsUEhFnPhQY%2FlUamb%2Bn9OXRVRWMOjkh8kGOqUBxd2i0VUZF4et0zKulYPPkQql7l3GcsJ%2BaClnObdhDMEekXT%2F4%2FG5i7PRE3mUibWtRShPPLgehsRR38OILykpr6vROZoUob8LlMKto4fWiR6y%2B0xNwGPCl%2F96C9S9ZDUsEmVkpst67ff02o8BhFW5%2FMZXiJOppK3fuU04griYGb3J3VvrY66J8ePL0TTdp9tJh3QCrFlNKBJ3csr6eNNGuArUmE%2F0&X-Amz-Signature=dd494dba5d23d7d0feda3c1eaa29b454da7fabf1987646390bdfbb72e961808a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

