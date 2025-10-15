---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZZLTGC%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAFyg%2BofLeBav%2BCIzCDtnlDLJHILj%2BnosPT%2FdTQV7BjoAiBzSD6mqmZWo4h6IjKjE1gIFZVyA1Qo%2BPECGTv8zcC%2BDCr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrgxru%2Fxxq5jA1kk2KtwD50IOA0egredVCBdmzF5scZgNQ867zUR4HUbtyp1c6lbqwOEr2Qba0wDD0i6iX2Q8whGl159KYFnfExG3Ieu80wN4aKCaZcPKKZwb14vEQB7K%2B9ncF%2Bsvul59Kx3rXLssi7WkRNa6%2BpWLOv89L2ljAmFmIyOxQPdUe9jxGdAvTS41DNIpR0Q0kxoujhflwA4C970LhC5CBG6zM2Goyv6ozUvtxjgraDBnWZPU5p74rrYuX0fV9UxID9dDmfgN7%2Fy2WCYQbI4yXnr5QGnA49dDfYTo4j5hx673LVM33%2Ffejulspm3MZVYcevVMwf8CmYGF65rw7SqSkEWuQknCsjYeL%2BcNaga7gnD0brN0WUYVRQsKveWy%2FG9rbbXNYVFBH7QjKpN6kbgmCjMKZn0gmBjWT46E9l0Ei8v5dkMLOwNf8rd5gnbzK5nfkoRzZsqEg4jEEqxQkBSOTb4bkSP6ocQM31AIO5qgN%2F72p2hKsFPqvpV2jtGSnjqAx43X7zUKILL10xpsyiKf%2BTWqMhITe%2FV2r9USqbGniVABzpA0c1GLj1W6qaHxHyxYD1hSY45POp3Lu%2Bjro%2Fa2Ps9H%2FcFRqE8QqNJ15ghTwQtb%2FkIkBDtga3aFGWRszeV9x96HeSYwqIm8xwY6pgGBKJMqtkrvMpRpUEbpOXeP4k0KjsP813KUjgvR22GD%2BzL6wSiSKnWaq86THtphcjotsPMMUw5Wpr6vEkoFu0%2FpVXWIPDqMqgCgehThp8pn7eleplIoxAaLwcs83%2Fe%2FIV6o5QZAm6W9FbzF72abicNzQi4FNrTgr7dXtJ%2Fwrf8mnKbewi8pna%2FKksHfqab43vQfgHUsiT7WG5YtrDKAwYo9FZX3gdRk&X-Amz-Signature=ae34320c63974808141d996311ff2fcd3dff97d541d78628031920ae53e61116&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

