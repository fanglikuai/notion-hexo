---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIMTCP5B%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIAUQLq%2FsI%2Byp8M%2F%2BNG2GFjSMgP4jI%2Fhq6kwLcwP9ieJIAiAAt54JIjX0P28IhQyD70PE7YDlIk4iPYjKtyueMTRSkSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwT8xFa8OjIs2K295KtwD2wz%2BWiTD5iAtK2OlCIJuZshM7J34HSZFW%2FW4R3BM59VWZC8Q1dJgf2zGF6bgzP6ao6KevSsPlPwqf6qBCiOpon2lQX47HyQNYOInjfiKX9VvY0vJZou7yXyUMYyWLsaXQNTciRgmYbCcF30om5MA7oPP%2BN0y3C12Qnx4Xi82Ex8T2FVrFi6J5FGGNvohLX0xKSbuqRER7yYHeqr0Ms04Ah9WfpTSL5%2FtWp%2F7n8Dxli0iAUdTPji10V7JkZwCNcYffcbvp1kcv6snXHjL8Mks9Up0T98HcPR2uM%2F123gPULnXVZvmkfmOVvG83eiN%2FBdwj7G4bK5%2FrzeC%2B9S2t0EcGdFFsK%2BkeOHvnJIlmzKZVQAMk6c7WBfmFUyCKniz8ykLj3K9zcDli%2F%2FrbvigM0gm%2FrBNDFR0xa7FrY1%2FJBG6TYuCIazBvg63ZpDGVWsCvF%2B6p0%2FBSGd6CH%2F0NfVdj1ZyXRgea3BK5WNGLMgrPopgwsODn8fmiC1NqpxPVGAnqfWhvwl0vVgjjpJwAY5LhIOI2zOlbSvpDOtiEC38HUA1T0uKD4dZki6rZ08esi6%2FHc%2BI3M01s8KrDBWxYdYYa2pTkgKusf3XpxEPWhAU%2F%2Br63dPnbEZVm6J5QDQJUdswhMDaxwY6pgGSFYSWYhfu4NqOelkaKCuOvIm3n%2FLuavmRhRKMwTWBaCmJ96wSoJVTEQio3JCjAiDRUr%2BaKXWGhUa34dkX5crhsbGFk4xWcBa%2FmuOOYtJTrKHJutj4f1UwwJfFmbFPTABk2mhXdvLZxduvbSIP097JyFRYrmj2vEhRh8KVUJYsmLMqV8f5O9xV5Jq7J7gPLhqmeRyFyTJS79%2F%2F3RIg1%2FhoMZBSXZac&X-Amz-Signature=74284a0c93de90fa0c0a768dfbce1c638cd353b493a1dbc0210d14ea66c73fbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

