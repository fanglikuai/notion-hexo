---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWL7A6WX%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5Ii2LKjYG%2F6nL8ThOHdc6kMS0I0tdmFudZYzIFAHRmgIhALWLe557%2BL1zFRuhIucEHusZuY29FPfh2juVTWTmxGToKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx7c5M2Br%2BeFQJWXfwq3AP5Fc%2B50MugBjEaZIAFFZFIrClAqaAhAmzBw4X6aFxqz3J%2BJoLMfAGPjvTb5Wp%2BMFxipqTcqyudENlAoa4XKiPQCos1X4nCw3FGwShpswtNBsyrJ%2BwWACfC7n46VHTTQSahe27zTGlyZn5uSOxuRDPrUdMApws0bC6hgwjd3kn1tvHcR%2BG%2BROYKa52tsorDvwU2eCko03YWBHRlGIBelS9DD3E09uNxwWbmGmpsPDaalD2vVy2Mdby58LoPEmzmd6LLpy7l5ReCCXQCq22LgpZHrwLlSDvr2OIM%2FrzJbvoSqm8LN1T8efun961Fz%2FQNREbykEysN0aCiE0yyrKZKRcnQZXWZiIIHYXbN2X75%2BtsDrzUk1MVfQnXdMYuDSAvBJK9IyS8j4Qbma5hz%2BcGAtmtajIX%2BvzYgFz%2BBVR1gVVHr22zlfhzTu6V3C424Tp6eJGRbecpvxN3AcQWhqf74I5SdtOaByzX6RUiQBsMkQubHTS0LBHeiIoY%2BzPLgmTyijbPxhqnkhnLxPFYJ%2BsnQNn6YLZoJaN7CdBLkMUmV2hNk1zYtAhp3p6hjN3qFFAFbdX3YLMfB6WfYH4Ek%2FdZ3brv303ssch56Wfb4o%2B3y2JhVU7tI1QVoC%2BDYI5W2TDe4KPJBjqkAdo0WOVca0pl6noyobC5E1SazZNKyMAjgABWdbIR8OYZp4QGlF%2BQ5Mh3TgfZrIV5lUi1nliwRiLnkBhcjLTT3PLpzx2iYlg3Ytglwr%2BNtDUk9yJPO3Fq%2FD9Eaq7KQT5GufXmORwmr9nCPdcIaJKO48xwwXC2IjoYVS1DTyDEMMqWtvZFRzIVaTaFwQgJczmgFqaJOltDQ2%2FmpzG6CiKhhlCXm613&X-Amz-Signature=84213fade30872b8ba6182c1127eb99e806a2972f855207ee447301ab2e20365&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

