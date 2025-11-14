---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QA4YTOS2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5n94yeCnNF6U3Bl3mMIWq9lAA3TnZH4CxCaBIFOTSQAIhAIetz4slqsS5KCi5qTmC5kwE%2BiEjLmAQFC6F9T%2B8%2B2TNKv8DCF0QABoMNjM3NDIzMTgzODA1IgytTtnZ10STAkiH9bcq3AOFVJfP4dLgJS%2FWXsLIMfMb3ucPqFZKLEfqLu%2B4rSvUuSvUNHqrRAqBQCNcVWF89kpc8TE0tObOF6BXNtr9Z25eSuO2MJINm6C0SNTfUgeBuE5P%2B%2FGrHe0yDNF9Zz20mVSS888Jc%2FdfPbUDaPScpPhz2fBrY1BKOaJe7mc71%2Bk2LxmG0IDlxS%2BaWwGu0apxEG2xxSdb53C3CA1ofkZbk%2FyrK1FciL4hT8bqnTnxPWI90A5sYM5CccdruffJOAOr2YpTrkJwGbt1CaBj0FbOCk%2FpbplZ4hyiU%2Bmsn6iWHRDUW1FMoOuPL%2FifsuZ3XMzbK9eQSj%2BQYoQx2IKoG8p8pDCk3C0T6MgMPgQqDuia2GgTRZHB1IQG%2BaG95GOyDhw5fMqQfZA450wxF4TnhY8HvORmNKNBzp6pFCuY1CaFG1sXLcl7pwcddD2SWutztHcxm6wd3my%2FVH0t9Bdr%2FBsiM8GpNxRYWF278nF2lTHg9Bc8kyjyvHNgHH5xfEueQMXv%2BB9v%2F2%2BL6VLxlHxRZU0id5xQihlJFnXgB3z3djVerjT961SWgIu4wKxMiYk1UzYS%2Fra5Cvm1hp86POYYobJniy9XzLgduvRvwLFoxdWqTccIR8SP%2FXNr%2FLIyd0bp%2BzDe29rIBjqkAVcf%2FnnbZj21RV137LJm79p6GOT%2BBL8xma6465j8KGHVzj66PKt6MafH3%2B%2BWWFro8PfBl7aNCIjBteGBwghcEaXRJVyR%2FGuZnRWXwQRtPEo3M53JXPOT1IZra4Ol6nn%2BCzV0l2t90A7wsMffh7HsQgD0qS3Pdw1X1hU3qxrOuEBaPM6xVhNI93%2BIWFoTHXpNWfdYzdxJki2k6Nwt2p%2BPQizyH8IG&X-Amz-Signature=63456663ee7a549145d7f4fab4daa6a1a4ee5036efe9008289cc23e4a28a9213&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

