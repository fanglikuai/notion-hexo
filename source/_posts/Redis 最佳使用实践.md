---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664N5SYXG5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T030054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCICZkHplRQ%2FnWZNYJK8pcldwz08fmAuW2goPTP%2Ffy8AgSAiBTKfS1An5IUuZB28sLW5BCeWXgxQcvLpaiLFVFRlwCiyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMtG1jxBe4bsgdXp9pKtwD9Gee1%2BbrHGquxEy9mTpQgIBytyz14nOc6Rcnzntj8T7qKOPL3Iqlo2kDmB4rLvUG%2BaFuYm%2BzANvhpC1Pz74oZzyOM0y%2Bj%2BqaM%2F98H4vBr0SaMmoZauJukbQEUiStY25hqHP4syGfgBm2woduCCMIY5v6%2BGK2ubqsodrSymC2RZ4bGk0GNXdtlMwDDUAYiUzTfRCoC%2F1r%2FHnugg9c6uuPVCVqKhj6p6ycJ6iHHss%2FvO1ARo8OYFw77Oy3HcZ%2FVEV5JocYxl1QP3PegJMhqtnbnymoXsc442pDtQNC6Q4i4Oh54%2Fnv0wOKCwiKaAYMmXtoXAnc8MijMT6CrZss6Pqt4j47ydkxfNFkJOuXX7nuRzyc%2FQlJYDP1oE2eVppXMI0gn3SX6M0lHPEawRrZCq2GH9QDKmFPwJDVspxXtUZOGLKzFsO2XUwWYIhx3kf%2Fs5%2BnouNIp51VE7vwdO%2FVvDXMFfYbNbdS6KMyVm9do%2FN%2Bhj27OA756hBi86AndzKepb2pLAjRmw7YinX7V5pgoYRR8jqmOdypoeufAGsJjvG4w%2BiWXKaqU2f3wOtgqF%2F8O5pSOKxQ2%2BC%2BGozD87qSpXhx2eXST%2Ba%2B4EYzF%2BD3tbEfLlUqkckNOlQvtuD767YwzujgxwY6pgFGLOpaUVXnOSjHmgXD2on5vKx9RP4JSVxa406Fux2aE3RlOl%2FGptm7xecg1QA%2FlYYEbw2QE4zV4KCqGngpuic3VDtvEK8KrO%2BmIqNlJt6zVBPEnNopjFv%2Bu2SuPIKZVa%2BI%2BonGdLfcN9Mrorhil%2B4D2IQ1HRZhmje%2FXJ3TDxiP%2B%2Bu1GB6nht3B%2F9njF97E%2Bugv6lToWvW1kT6Dub2ZSkSIkTPuUuTv&X-Amz-Signature=909484032bb9aaa76df743fd84cabfab4cc0dc680a96f4b9b5fe4503f8f69af2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

