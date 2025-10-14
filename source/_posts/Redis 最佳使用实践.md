---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXQBNHH%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6OTzKPCmXnMFxmv%2BgZtHpUgzzvVB2%2BX%2B3u%2BxQzsn5NwIhALTloXeoZQ2%2FboE6yCRuXO7b5VxvEqwr8SD3PgbXA9fhKv8DCGgQABoMNjM3NDIzMTgzODA1Igxxv7tYEeWknM1nkdkq3APHXrsemT4AUDCozNl46egCLS39zOLN5soS36dNkqpq2vfDDl2iam6EBZHXmmu4d2fSwDYTbWGnz9kITCOOkEBcHjL50Xsv8RmxvkQR1yT9Fygf%2Bj8PDRBG%2BqaJaUZuLAewHvCHb83T43oQEGhHcFUiCqh9n6xDIDcE1duWHsHDcL425MD6rriYwSydvBbXMRcVX9PvIEnOX691LsRTNxz07uK68CJQt5L3R5gN84gCEekrIe1gVt8wYvUk%2BZkCIQsAA9JmNrCx7%2FfaXJff4LURDYVyTktY%2BMGcIjaoAsK%2BC2XwrJ1wOF1V95z4vAZ%2BnA%2BLAC7ppfwWVLqfEAWEESrwHLZb0xwuLa42hhn9uzpIttCxVG71%2BWJZ3GbKNfjzYELleVEQa84dsLsc1KGlbS4tytbSaeY0zou9RrVS5zdLqGOqhYUSnnVCkVECnuUWlYQR3ygmNhWHvWaSy4UvedTeACgyK1OA5g1H43KD4I1su6jpQTZcl8QRbi2agAHR8KpKZsxS0IgBS2DZqKZfAH4rhPVFUB%2FJkmBq%2BHHML3%2B7udDznWUxOsRR%2FeEhXn9ZN6V2r%2FsfT5rWCeGEIg744ZxcVnwUxhdNL5%2BBkDJLevHPZNKbxFI2xc1XTxMkEzCkprvHBjqkAYBOCKDIO8JFyqvvpQsGE9qSM%2FBaOvBXNaWIi3IGUM3aP1KWNjxHwQ4gQ%2FEIp76QY2i%2FEF7VGE5nnAbidQHE0uBGW3A2l8xKe3REwcXm56szDsDm630xVQolGJh%2Fe7L2FJs00N1MXjI9upoX2ZhwnnxhQSL9YB%2BNhu%2FS8I%2FN4IncqTcH%2BynhnZsFbzdX9HEkoyKxTy4i1Xg9%2FAyMVgloA4Wcd6wO&X-Amz-Signature=8f0c8d5d2d30810706c6945ab4e3b520de6ce0bcffe002d4944cf91ba696d906&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

