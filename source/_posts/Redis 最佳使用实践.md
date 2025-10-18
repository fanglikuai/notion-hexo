---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673RAWY5J%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQDWLThJG2R89%2FnxDSGKZQ2EAcHKKp2gvR6AjSbZudJSXwIgPWUrcMrUZR%2B9nFvEbvKg8dCES%2FZ600ZvI%2BXTft2B3loqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNG5Cg2f435P1IoX9CrcAzN4zsLH1A39OM9WK6ou%2BxLOn%2BZ16fHtSBLSQk4MpxWkhveXQdXY7PSNfWxNOFmBjVXFFXOmEEXZ%2Bw83sydkReTYRKLZvS8ThUqXhTfso6b%2FFgNeNxXexaV5nWW7fyj2y21LcjJzVb75sArURSEpu7AirbTPuggu3rCdP%2Fl6IfEOGOVJR4XVIcPF%2By1DkywrSpAqibCEVfUnzKN3PBTwAiD4xXTAwTFclHZQXxa3ouy9%2F9rM2jF9cFSgrccq09xKo2t7OzKHcxktTxU4pqDnnHkLyNFgLldjJyZPoemZXpTvuqtZK6ac%2F1al4VibVgLylLWD8bmAy%2Bc3Ouiby0qzGWPzsROJG0TCfM9V1rPRal56S4luIHiWgZkOseVrYBzHgD5chGHvnz1g6twYfrWVYYGeBwDotLRPTr2TX7aQKb0S2PtLf1hVm65sTdjOzuVhVJWKXcLCbMLQ9o8A3pIsrfBCaG5XA7tZ6W7oH59o8mzkTCyPsdO3ydfkwfoseuGFhlZ1nzphFWkO2mdE3M66m%2FNiF6VuqwwNpQSvbWCCRsJMqML7V0%2B%2Bl6SOeD2n7J4Je0wjDtFS%2BFIHs4GHjhzxJqHMVVPgC2q%2BDHwrWm%2BHO4Cxvgq0enPjNmtKt37gMP%2BCzscGOqUBJxoy%2BzHjPT%2B7pxOJUHS4KITKaZ6pEKy4eCWfT4x8wwYduFmr%2BvXY6FPTvdUnXE2DhtoACaL%2BNPw0Shv%2BXm9VJffp99G8%2FFCclsOcrAZNBXJxC7K4JKw3uPNxaaxvsf842Df7JtExidR%2BkiGF4glPk7lP9p3FsM%2FjM%2Bx%2BwC%2BFA%2B8OFlfyw7KnXw1eEn%2BJ9mTw7QmsfSClDNKFIMlsS%2FFnKWvvYdsm&X-Amz-Signature=5095253016c0218507cfb3438fd876de25f326f7df528faad3ffe49ac2882937&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

