---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GVASNLJ%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIBCGp5%2Fh1yZgrG3791nGRIggONhS075DJj7st3Tuz36gAiBuhLJOcSaXgfcX9Tr9vFyDzict9HNXHq2%2FNSPKCulZ2yqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6gXdJbZr1kjseWlYKtwDxQSAOh2ZuHJgRbEYCXiSZB6Pm%2B0S1AevMrBtpLJvAkEXkRexuFbAFAdmhT9e3KG083RmrvS%2Fy33M1FgODJwcvuY20fP9bcew2V8ckvlBAgwGdMLLyZriD01K805ZKIOhoPP%2BjJTiD0G2ceoQh0%2BndKy0qUNi7gLOv%2BJxS%2Fl8cQvbxtg9RzuUgVCw1AbcKgvKwcykEzDM6k48W6hfC%2F%2FVfJKbdI5MqElbFuaVXKi0Thj4Thm9JjJwx0VYOPL5jLZwalIvL8o9LQqSvYlul1DEVAZZS%2BpDNA0QbygHmOd0xZI3f8fWPC3H1fmrT7UOi5l7LDt7UGQzcnX8g2bYJK5Do6qpkjuZaGwXVufFK9oWqf75kULRVB25Pwhmj9qX3SViHs8ft1jDoNdKMQHnAVeWeL%2FjEGfafbfc3mDBQZpwyVVYXwneRRIU4GSPy0S0iYAJxvCko1AoasacyHmX6vQfOOCNFP%2BHNWA2zbQA5LAfDbUnuGJEuXn7LtLfDoBai%2FDWn8UIRzrvKGGpx90HuPFTc0M2NAWSH4r1%2FwunaPPKd4tkWLRpvidFdT8Z%2BQhWpGuRDP43eMUG3MOBjISf1uzJe9J1347YlN5Wa5vIDVBD14g%2FCBqIZwLNUz8mLJ8w8sy7yAY6pgF%2FNEuxEXm%2FtMcPxQP3KL1rSPZx0SkR04MyAOeXE40fKL95t3v89x7TTDslNmpiWDFLynuFq4kPd0NjRvpFEbiEnHB2%2FKeSXvoVXfO2Rg5C9DwoJcBkQzCXgBCjgyUE6JuP6%2FmPrWwBqpxb%2F%2FWtacYqY0YVMNOeJCOR41kug7X8F1y00YigbjH%2F8C5xw33eeGo9%2B%2B1PQ2afp3IpRm2zCyzbE3usmqic&X-Amz-Signature=bb15633167346289e053e37e442fdc97b1285cc42293156e9581c43c4faed44f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

