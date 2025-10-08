---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFY6JKO%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIE6hYri548FmR%2B%2FHCoGBTfAs5cL6cC088pAP%2Bwhi7gTdAiEA%2FbVDU6rDcGRoD6%2Fvsf9p9DL9IDgHQfiTEiqUpZFM9gcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGKysoQBqDBIOHZKhyrcA6hSFsY9UHIod3hq4bCFJOnDZ9LhekF62Cgi958XvkGFXJkQMgbKLCgAOHi0SyKpoLi92CNqYPfzQRyfoBO1TDqhAIu%2Bse8k%2Bg68uy6K7%2BrQjEtdIM8JPobUiLKX90ZRq9r5wvpZFs6LxtR0q6rUnBcB6mYQqig9k8uwp%2BT9ABHYGmTKiFibjAjS8XO15DZjgZDbJ0rCP0Rvkf%2BrkWI2fVDrtVhU7uS8Yy8iwUqWwdKlBpKjBtGXEddH8x8r%2BhZJK7PuvkWJg1820cBnOSIeXXkFK%2BkX2cYR0OhRlP%2FSst62Z86oskPBCOxHvOqLYLbVL%2FB2jbCKnvSTQtf8h%2B9g3PcxHxTxDazwDs6MwpKMuz8iHXxAz1bp1VIJigWt%2B1nSrcu%2F7ubjDSlIwDkPU5ns9%2BRao0WhZQaK6U76WI%2FaXHKAKoPH0JzVpD98Ng4%2FDMvfE%2BrTrrno31vRoKcVEGMKALnyyOJAyPpiA%2Fs49oexaQYLMP8HCjL58hAMnj%2BRr7WyIaejRnRvLxrIf3HMubbUttd4wDsDJXuRO0%2BYszbEXBfZjqzRnw9l73ZvZSN3gVg4mjdSA9XroX37ZKRt6DETqUgYpx9wLXHgK3u4dS5Dbg74Fih%2FmHLaup%2FcxdmqMLbDm8cGOqUBoNbID7edYBPboKd%2FCLupdxUDZepY2q4SZ9R2Acm4SkqD5KhPoolnkE%2BFA7AGdspLWJPeDgkA5dloX9JYIfZy%2B7mL1VlqwygEkecS0M0B5UC0T138l55HMXqFPhW6DgG1WkW1DXT4P0mjuA%2FbEWFcAj1hsDNH4Ov9fE%2B%2Bpd9gZZ4H82u%2Be6WlhpcsMCfzx%2FhUMw6VAOH6TXRRDkxdv7G%2BUhmMQOWq&X-Amz-Signature=ed7f3684f8ed16be6e183ea2f354e361bbc590c64cd3cebc7ef53e2fba16414d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

