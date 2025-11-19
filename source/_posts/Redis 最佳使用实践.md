---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JOU6ZQ3%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQC%2FdpBjAU%2FB%2BfHSrSz6TCwNWNtS0IK5woEB8FQBo3Q9CgIgeu32%2Bi2wq9hfDrEVKaT%2BAPPt3jbNAX9b2l7Jbv1r%2FoMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG07%2B%2BLoFeGRvCF9dyrcA5XirEX%2BA8cTC%2Fe4fCj1InLP903TOzswBbpaDoaiz3O4aF0JCu4%2FUjPmzhptyMs6xKCJArzJ0bsqgxBtne1qFcjB%2Fcst0%2FzT4C4iBpQfdbYnAd1Hn%2BP5V%2FdhlJ8E8wSKkyZyf7J%2F6Wz%2FER6yw%2FBJdDuw1fL4g4Im3hbIIFbj7%2B4SpWLZLAk6iYqs27zgZVZW2PeMZUkqK%2FvSckYzp4pffYb0%2BQ07UG%2BRlb9I3kWPPnP5k1UeT8o23WGkeEogCpH2S0IE1O45GhaDXsdRfzb9Ve9n0cOQTgu4Y3VCTCK3lIBIZlzqr6N8Pjxow1Z3LdOCSCeXaEly5%2Bv9%2FnNFe4gL%2FkBSTgVnp7mCqIdRxOnFNnD%2BieA3WIT41fWwSHbu6qfkNDC9NSYnkP1%2BGY2R89gQ%2FZXuZ173NtUe1tauJdKSuS%2BQ3RV1ZeahwhVbk%2F4vYRaiZw64xdbLfPy69T9VBjXTlOvb5Cd3li%2ByKtEqi4%2B16ta%2Fsve5EUhRNPxfaATLNcgR312BoareTlZV2m2lLXD4YTpwiP6NN52GwR%2BqwTodRs%2Bl6SlvpLgElSFg9%2F7b58ApNr0sU9WfIx%2FGRrR4mOhy0hcX8KwBbxIoGAkt6ewDM3UfFo59%2FGMW%2B%2FyzvSD7MOb5%2BMgGOqUBBWlmoNyDzmk6pFwYgTVykCOuISueU4U3db8WsUsyWveABNZZxUCgEh8noh58D8EXSvZhzlN%2BXKw%2FWVtn6vV28LVcaojVBHtHS1O%2BzROrWBhW1DGK%2FSaed34CxGZLVitAnzVn4RzuWK0%2FeRkmYFn8Hc12lhz7sFEuNmkIsAxQuQuDBnhL5y%2FzBmlFLKZpsYDLgbwsnOcPm43p%2FlJeiROWbrP%2FKfY1&X-Amz-Signature=b88f6d94f172cb0e50aa6f953b2b7c9696a4a07cb28441e8f2b7fd2d099a22fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

