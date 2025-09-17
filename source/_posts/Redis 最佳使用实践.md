---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FDT2UPL%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDnAhyIks9GaygHFzoUOdMStwq3xDLC%2BPm6jCtOdRGR5QIgEXpoSrg211cf07oIG0YBOR6wnis9Fe%2BwuFgKlFvNk5gqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEx7pgX3O%2BuLQh9GBircA2%2BjSLq9F%2B%2BK%2F%2FEF%2B9pRQy6NV3ppSrGvE9vTZgCl0gizjqatpUkqmf%2FF21CejqtzVhhciZdAWylXW8ymk6kVV9gLOQJD21OR0mU4Fhg4bAtb6wrvxTXXD1AmN%2BSIL%2BZ%2Fqh1CdsaZXT%2BnIJg%2FnQm63KywXDjCkUzCtrylRpw1SiZtMKJvWT0jBPTrdweEqsfuISJDA3fv85AZbkpfT%2BfK1FkAj0oJg1edUn0tkAwv6Znv%2FXAj8CE09JQXEosqcHsN7I%2FNA9d1BJVKXbkir7wmtcaZSk%2FJ8XfWc66BabUagGKDqNyYuk6abNnV98Ixneik0nRY%2FydrQgZk2L2TtUpBRw1Ns%2FHW%2FQ%2Bv5FO6nS0bj716lMpdV0mjbNBTI7j5Rkit6kPjcFo77vUlBUL%2BMhoNbljEvW6kb6MSUOcMPwowNucqW3H6pPHiZ%2BJlg19vYOy4T5Y16YySkB4xLuIbITLIjIeY7OkJfkDqYXmSJimZVimBfh1MWIblp5o5STvQnv0k%2FmifNfE%2BPubZYSkaPUwOLn7dn0z%2FCxauZZcoY3g0sIvxfDSTRZ4uhmS5smRqpxYelJTOLcXS7xpJzgayHmJBhOMtiYDFUgYau4UJSNZ793yH7oGjs7zJDMRZTEhoMPz1rMYGOqUBFsVyuhM1xlgJaBnbAmapjJebr%2FNOTEN3GgSFe%2FzK149YVh2PGPZPFf1CtQCFXbMqo8oS8a0Cufe%2BvGEw00nitsidC1UQRZTn4QXa0N7qz4d9nADmTeeXAenvUnTGeZAsSJSYKhKwL1J5S9mucf%2Fo7YKLyap0VmjXVSKuEBrPQrz4i%2F%2F1xpJVPx5FyTJ3%2BDS%2BjokpB%2B3qcKUvSi%2B7SokGwegxEv%2Fi&X-Amz-Signature=c61deeca7c2e4e41e88cb7046f28109323b70f64744b433b82b945426d788268&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

