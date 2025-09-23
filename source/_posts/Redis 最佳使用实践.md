---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTODG5D%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpVZgx2XxlX%2B08fHEhlk83medYXiVdQ%2FWQ2OTYjFtAzQIhAJ3K4wecIpkxi%2FAkTTosonoUPfYSg3jBQKhNXUG%2Fb%2FWdKv8DCE4QABoMNjM3NDIzMTgzODA1IgzGZ8fz8PFVJD9DyuEq3ANV1vvBRSbpx8nJTcwiCwiGTdu2ssYKIGY1ghOk5hOzhyDkG212mJd7PaHNMSnYTm9A9lcDejmXU5VPm3P5TVmOpKd%2Bs9kI%2B7cePyWjCgZBdaOdsQShJCVbNdsTWSpwmS3QE%2FR%2BGo16KOM1ychzMudj12jqAT1YGWwa%2FR%2Ftgxned7XIVpo5qeb84QXegmlnfu1lzZhIuNQrDkBcgoCQIwha3yWe6WKtUF8e%2BR3A33CKK%2BmC4cqNSWunEBM5fwtO%2F1elTeQaqWH1PnXpFenxitiFg7Spw1LfO7Mxnf%2BpGopoWwj6zGvh5%2F48SPCJUE6J%2BrmUfZ4Aq5C%2B9FrP4GdfcWuYXumS3BCkROOphOYj1ch%2FxNjT62fzDVDO89oxzrzU9GBEJTAGXktLWamrltmiUD8T9nD7nenxzOV74GzjHjVQC8Xx7gmVMlpfc1mt5iR53XsNYhEPuLfctEi%2FVkilvuMh%2BDIR2arjf1phthGYoUygW%2Bnbr4g4fWUAZybbcW%2Fs7WdNJuaQuxg8HOqCoQU3L5yZacpEIJw6iAhqSShgfdvAJP%2BAId4CBqfZXCCs2H7PsGHd0rrrDg%2FYX5SLotTU3ziOv%2Ff8Xv%2FAPnN40GM1u3Pg6qn5DMCh%2Bm6POjeM6TCNnszGBjqkAck5VSy3rFoju4l%2BMPqk6At42%2Fr%2BsHQkZRqyTWQcm0UYwvh21Wf9rN4Oi%2Bflftjbkg1fQY%2BDLKJrH7tk443%2FTH3y4HJ9mk6jrgmp6AEPxRYXYqyRrTDInZGp%2B396uQs%2FHMkcDtmIAvJex5qPMudmNzsodrDqBcvCRa6v4kqch6dXWOqoInF2%2BvzQ%2F1UQcupyZ%2Bl8BIkQkBqUzhsyEKFue9IbiWR9&X-Amz-Signature=5ab24c2cdf37d42b68273427e56b005c56c9f87ebd5b803ef16965a0dabeadcc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

