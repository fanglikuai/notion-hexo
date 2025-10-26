---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ2M3ZLM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDA9Wz8TXS781U8Jiw6IF1XjbmG1CEhk1MPNGn0UGgMeAiEA6CEQGQf%2BnGIkoGbOR3Xxl00O0R2nT4dMi7N0WmP%2BG%2F4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNutSz7psuMh4bUy0CrcAzkqluRfOajj2koG1IPsgibcEcirRcKWVdovmFnRau1Hg9QOueJ6%2Fok61WOD2%2FxKEN%2FDyBeZMO9FF7tokJzCXCvOSoOhrPj%2Bp4yOYiauuQ%2BEHJg9nGNP9IVIGlGhdOBwN%2Br34kGjicC23vZ6YVKIqhIJYuS7giDNInSnAuaLGxzS8tVAhmD5FEaTsgwFI80%2B26BpIltKLFacisBSILjQV%2F5cxWT%2Bw5jVUb3%2FUkNjJQDvj3dSyw4tD1XUeTYWezqLbvUdcIvT4ROPGxDfAtpyIBJ1psyWi%2F93Eikl%2FlWVgiiSR6oEr2TXPTXVNuej%2BclpY5BpyilHM8QAn9U65xSu8r0aJm0cYJ3Bigv7KvF4BMusk1mSSJNM3%2FretonkIZ8zz2tQ1wa1ZJeV%2BMY95ARJ3igwJBQjEmpk9oj%2BS%2FtmgSPf8TqsvVobsdrFhYo%2BpsVWyBqgRD5HVN5nvgC2EyyaSiYs2FMPu5DzM4KYjPA08llx0Cj7dVfGmwiXIXdnpQEVRh39FB4jm24M7b4DrzA96dMzSgFDF3SbNchK18jp%2BQNDcSc5poXJ%2F5m5jRgXFlYtzpIDU1fcr2Ti8%2FpNVZ9fHrd%2BkZDBKE2D64l3sxl%2F4mc0m%2BkeXzdDY1MMfiWCMJeR%2BscGOqUBtgne%2FUjd%2F%2FB4PYVUdxzFEDmoB8k4AbPNgxsnXCX1xpPls3N36Mbq07MR1Zze1igsVYNkptF5honuyyIPBIArIk9ltR9oXJT1ty02Fvu8muq%2FsOgOSm4nwBjRB4tBjOK9bOQKoPvcm1Hmka036qPXcTj7Dy8z6VX3WqJQCaXECu7ZF5HwtdFG35mNTCpRuPX1tuVnF0116OLkiq2139q1yYhUHwhA&X-Amz-Signature=ccd0bc54cc98cd28d594f378bfd3679f12ff5c8139a5debef792e4c7f94519de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

