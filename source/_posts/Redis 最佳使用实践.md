---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCXX7T6X%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHqi3JH74FPJtbV5yvwDnrdy5hjvHqEGetkf%2FIRqmEC8AiEA2JkXHu03COhEaZXHCdWft4q1nfR20PqzTZYb9d8l8CsqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGA5cPD96MNAzhdfYSrcA%2B9GQryNsCvKFVKON8h69AaoLfg3K7lkJNBRkEabp8NG6l4Yep6TeyCN63rcTGXG0sEUcIGTc48%2FrYYVbFGTbyTiA8Pygay1J%2FhBTibEFpWTYJjChh66O6Ez9P%2BuvD%2FczxgglwFieT8M2LGgNrPFr51LYJxH6IT9W43d01fojDAnE9oXR0md%2FC%2B%2FLm4VdN0ex9ksw1QkxMSdhLJO1NXKJZv7WismqgJLPC1%2B25jN6IEIsWPjOXetx1vFKUumlsu6h4VhWdLCOhYUaQQA61WNfILwVQqGQ778kw%2F%2FNqvKz7TUDU9oh3WWuZ%2FZXF%2BgUrVYBKkwtwguo5SpXgwZqhDVKYzZK7zcmyP3%2FIYCHh6S5Pm%2F%2F%2BbABnGxnvCeu3fqfWcvsJlps6CiwuNicQPk4jDdN%2FYQkz4A%2BigGX175gP6CvWVeZRVQAYLxC4cN0LanMQvd8Ba6piOoM6StRCApdnD8DJGOs5%2FNq21flYz2JYqsQV8ujoXj1vKWcF0aeYLA13b0tU96SZvJ6jXX%2BevqSixifsuab5vMIMkacdsSSPwhdnXlXoXAMBDAWpxl4EuSEkriIGLMVme3sgC3DS9rOAZzgdsxFIR5DlEVkuc8S4NGX9p2L5O4VtlMBet3aatRMI2iockGOqUBwbZxWll8RoutxXMK1E%2FEF0HlTOuCwBQYSD4T2OVEKz5sqNcHDMOUlbhPYnwYERUyL0WNfdSC%2FvrXItEmUwKyHnB7IJ9YIZJ%2FOtc2c%2FMVgpdyjR7P1uui5XYmTF1Y9Oi7pBNGRzhPOKyfNMb6x2bT%2FAja2Pe27QQ2VXzc6wXUvCFeUiwViCGVy%2FbCoLAzFbJf36y4cqzFjb3%2FGidfV1ywJon5oGfO&X-Amz-Signature=30a09cf9320ae15cefd7bd63c769bac3e422643ca8b31be06ab2a6fd0606bfeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

