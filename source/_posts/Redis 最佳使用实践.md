---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYX2OMR4%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDU1%2FJlZbuIBeR%2BfObX8%2F5p9HAk5UYQZSleVqOJLanY4AiAy9h7KXGgGPu0hbQ9FsGnzLFMhJTMRn5ZsR5%2BCubJHhyqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMgNsFuKeTlZmglRKtwDvHbMUfO8F95a3OhfxnLKMsW3ySJnNckNJ9peU6%2BxvTvE10IoC6GRGIrTfwHi1X28u3WWB8rQ91UsVH3lt21hTtB5vKSHcG8Xk2S6p1U8c0VqfOAuVwLt6JCUSSSlhhFJkrjlrBC8kqy3epFOfC7AVRiMy97FQ3sTqpbQUPLCMsq5pGbxWRquNMnd3B0yfubLh3yCpLGw8Ta9TXyLhLNTVYjET4f7vcziFSAEeH%2B%2ButSrk6ocBFWJuWhRp7Ka1LvOLBsWDv3BHlt81IAnrsZFTCv1dWm%2FynEDTYkRYvKgJVSFF0HIhrASkbTmJqiOKRD3Z0NB4Y%2BcVA8hQq9ZeR6xsAXnqlMZYy9x%2BjUwII4sXdrahNFeZtx3UWiD5eQUHgPN5Rf%2BgLnhCFrbQ5LH7h4aXKvHrxA7q2tvm42E1qlqCyT4WjQYKz85mTIK6su0zjTi0PDBIsK3T%2BVBbnlef19%2BYbOcjm6%2FYPr3%2BfCdvtuGvycNzOBPLRP1sEyGgdwoEkiGMo86uTCRTIRFrtfNjOr8uFrIRDYq9ySC5%2Bq%2Bu%2BFVeQWX7CQE7ilOZKYs2nGv21lrNnvuPhYVqX3BpsLOzH%2F6lEJsYw8nPbQAZoTYnTpRWSzZrHNw42b6q%2BTbpi4w%2B5uAyAY6pgEcjWrsxUpxhqd%2FnL2YLiB4CEM0FqIK0iUjELrX%2B8n3D7orh%2BmONdieu0pAOcFHhG63Ga8AvTCpIoIxIHGvHems4g2TfSBOEe%2FWriQNQ1GK7VajKRvaQZ9MSE2SuGIWLg9%2BQ2YglVmeK44l9hHTH76sYpZ50km7LKB675o%2BifLzhNcKvYHTy5y0cgGI2mZ7WahsmNFAX0dYATYblz1NmVo0bNCTSSr6&X-Amz-Signature=0e80f6a90aa36cdb0c35502809817a897655848d1a5577f2a0d60abf3fdf77f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

