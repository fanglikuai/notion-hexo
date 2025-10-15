---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMCF6LGI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2WM5pDNlm3BFIcGA0INBjy6IiIdGsi%2BUHqBnhfho6mAIhAMn6IyTpWdxU%2B%2Fis4eOUmxcrAm%2BSOL2DGpSQcaIT9NQTKv8DCGsQABoMNjM3NDIzMTgzODA1Igw%2F8Bx02FOQfXrlQGMq3ANiKlChBrckFM5Ognc9NqQB5UFRozEAkj5BQDz8yafJr5CAtN3t49GKZ70ffgIdjxNzaVyzBvA%2FtGY4uVAvtiFZCgBCy190sa7v2i8WWxf6Zrlruj82WROMDIH7zYnOd67gcA%2FBwvEIMhBv5Wh2mxu337eo7DVI9GI%2BO5XffeuUDTAkphFCiAln6wdRA%2Bn1F9GL9acmJDvXLdm1kPEP%2B7yuZcK2PQKhdA7fLqx5f7YkzL%2FbpEUv5aSzVSF6w199VWVu6jTP1ewdz85kNFdf06oMzoFe2OMhPlERI6vnMxxVApEkKI4j2fqMBq1ggzAMTegqVFCwYmehQUoWLU%2FAYGv2CEXYIo76lvuVr3phDGpcT4Y9%2B7O08IJh6XSSBK9WyfZpWXZ1t9iAyhcYBfrnLY13AoAiIjv9wP52LlTIz%2BiYcpS1cQaJU6pd0PchJOEqMUvHeu21raeCg0rfvffSvO1dg5hbFqxz1Qx2zdEz1GOmr9ICdIUjTx6QceZWq2n%2FHF5X6Aisi0yqMvy88Ns%2Fs6B94lTMB1MYWX0aoJOL2yKximINYr3hTzhG5uZlIQjVAB8RZVYmWw5cQMHUAg5%2Bw5%2FbZExsudFIRvEdfaeyUUyCPluGYZYUXZeg40qmLDCHibzHBjqkAePG10imlOd%2FgthToLhgmVcScBp6rIMVFVVq4ohZnlQb5MFTvkUYOr96tNJ6of96m30%2Bi8aDZ1LCB19yirsHfpZ0t%2FzQYFh9p8kFZ89XOoWpzacg5fyjG8EBnrhCxiifZgVcuxhjIFWFSZGUx%2FDqwFo8BelvBsrF56j0XcO2iTJEQQ66TPmilJ9MWmlLJ737e%2FHyac2xtK5vO4uIjHqLCRvWB5rn&X-Amz-Signature=4c1927871a295b75fc609293432047cc12d9bf5754b8022e691a24b24fe1d797&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

