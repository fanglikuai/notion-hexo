---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFFBJRWE%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCID4Wk3E3KqnS94Jj2Kuk%2BS46%2BTevDlxWb612nlvq1opOAiBD7EoNpz61vjhkU2d02LOYX8vDDQiTpLwwQpsDBY5DDiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbejrq1rOAmlSA%2Bh4KtwD6gPH2RDgD75VqD2P3OvPgOfJsA0%2F8wqzoEbM94XMxqnEjXTfdGGZc2hoouSeTGmqTySTaF3cPklfonP4RXdPMrtNjXCNXJ8A6PX0OS8jDey9rxJb95ryWcEN6zE9q4GcwW9tSFf%2BycfHm9Lsma70JoMmb0POSd%2FJX59F6cq9lyONZaTO880KMndD%2FbKI1mOZtgtFDTgXMbGBxNNW7eyKlXBD88GSrEtz%2FDz4MmtQZu6QcsiCsj0rmkF86Xz2RlU4DH05w6QekyqTgcRMhnQdH10Ei8KA5nwKo9TjjQfAyUwUbGWOCPAnyFqpHSqPp7Y38cLiPuu18lAtEJtmARSbBWu%2Bq6tWLs%2BgPhFGiunLLtm9xOVZqQt04FLk4LMuepFI9ODBqh8FvhfekGvA4d%2Fw0mh5H7uc%2FDqJlg4o0jPF7qlyEH1UYuFHaBd%2BkaRkHX2xcC2375keqU%2FYs2fCyTHX88R1KMi5Z9FI6rysKXXFp3RSJUrtkLKLV%2Bajk%2B5pb%2F%2Fja9F37Sp3IHV1%2FLLzA4%2BPfiVNvti1aN9tM75Y3d0wVWPuSvWknbZRHX%2BFjPlNOzlDKkZ88%2Fi6JdWXF0%2BXx%2BRs1w3g7vD%2FlIaELsJe3GxzvTZVC%2B2MrY4ltd8eq3YwlI6WxwY6pgFIdvLTk76Nx6NgA%2Bq1whzOMMcGIOmtB%2FzXQdRKJ%2Fvqlzw%2BTjn1WdMNopNxSNHshjSFjUinoIOiQketo8%2BupibipSgQipMvxIKKks9mDgd%2BsSuBOsDc7F4qqfLwyrY5P1Q5NM%2BCM5HgwibueF6XSwYuawql1ytFA5iwtsPFAOkSM2sCr0k6VSw%2Bqs3wTbbs4ii%2F2ucMHd762xw799hy7xZ4B02LWgnh&X-Amz-Signature=3c0632dbaa84bcedce3e3425acfc599862325892c467623001c193edf9f2293c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

