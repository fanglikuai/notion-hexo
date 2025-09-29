---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZFMJ3JY%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIAD%2F9fbrX5xvXL6t%2FnNi9dOitg0%2B8Bvvru2uW9wj9cqqAiBe6hZnCyWPbvmuQP4lmAXPE6lZVCcHX1yStwn3alNcfCqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2a5FqZg%2BouAVgNxyKtwD8xBMn75Z4Cw5Eoo94k%2FKg3US82NCSQHYN6RMdcOHO2v3rhzBVpvXiVyv9hBMXw8bIrSzrdjnPk5%2BAsc4CaITOLXs25Pj7mrfpWuxMkUa8UdGclnLIlgnAPCoTVOIPq8TUCLphy%2Ft%2BSB8pEL9Kl60zXPB4LhOIGD92RbLiUAWep0G7nboJh3S2fTTyX11%2FNIfaHm4ZUkugjFd1zK83O%2F%2FuUnTzdAQqlF2GwrJFp9rIRsALwjurvI9a1J4fy8x8qC31tfCfjM2kdhhCvglWa5c4U8l9q27l20qssGctyGhniHTe5VUg1lzBH9JXpYyMo%2FvmAKFVjiuizPoAfLl13xY%2F3XaYDnEk4Dlb1%2BnCjAaAcCtmbfJhA9RDdTJeVNQAv8LfPjL3KghmocK6CSdkIBsf3GCH%2FmC%2FdNZjk6ShebhvE%2F%2BPfCTFEdtGqrJnZ0OVK%2BmrrPKQjy87vwaxCSoZXGyOml98eIVuCzCZclTXvU1Kvn9Ih3BganYhTeD8xRU%2F4%2F8eaxbVvuqkQaJeleeZJoHvSJirtdfj0AXt5RI93gXUDvK4Q9C7SeikhYK2SiN%2FVkEtuKCSVu3yTvHVy3yCi9ygEjLWNQmy7eTkwcZwWwQZCWgrmleDBqlddF2274whpHpxgY6pgGQPl7Cju%2FM0trpdw5ahaYKl8DJaIEsh6xouKAEbEgBbGOAiruGAD32n9fgWaQB1KceUrdOk%2BSB8%2FPuXOZzbk%2FA15w5BMg5SLjPDlK0i90zF0todYPyJHKL5nxmYfDMEvppuy5dU3ud0nqQR5r7Q%2BGYJCLi2UGPkYyEZWJA0P6jONVo2NJWmtQRUOGijF7bKMseIkiPHfIOrJuc3X2F36HonUah8In7&X-Amz-Signature=a743de909cbc3d99846189ab47ff78f9448774e5c13004f5a8ddb3b6e779f1e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

