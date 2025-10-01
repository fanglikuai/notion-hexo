---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRIRXYFJ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCn9NKKpOwJMxLr%2B5eXUbtmtrmOv%2FYC8q3pYuuR8EOCqAIhAP1gYZ%2F7bMzfc5cTN1XFsPUbZDvCAa2fTlZbkTWC9YhvKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwhJXkxQkFDVwiqf7Eq3AMpDdjAnginkMF5No69VzbslXIc48Q%2FVY6jWIjpcvlEkaGhvE2LHZbzfmUmzIQxuR8hTEE%2BUG2bwnlIgKAUUq5Y4nkFQ4wa3rZ4NEAxC%2FofWmwb4UUHl4WiAI90Yse%2FDOiu02knFqnRagYoBxobXA%2B1UD06Bn73%2B18P56k%2BT9mh%2BfvxGLqi%2BtQG6T0hIwdnDVakD5cdniWLl%2BtRQzns0zHoGrm6HUyJtde4PtBfYjG0wDuVGmQH5XsaFutZrJtv8koquR7EuxPV81liyK%2FVhDt0S%2BuQefJrSHDpEbMV%2FXSP75vW7wixZjBlKCdr4BBe9xqZ3Qwh%2Bb%2BnohiV2Cxt3LA4rBoclgegKWG6raGHKJdve%2Bynw%2Fv8afvg4DpGg8WxxZY1l3hVstqhcXGHKwECMxy%2Fu6kSxMmandwAMj%2Fc400ut4oLJY%2FexzyyCY9RuQ5gUtajKRp2KThtlYo3eyEJ7kcam0jmv7ksYXG7sQ5IBkvp6kkWCGSpnbm5tLipJt4tqbHKDm0lYbntE%2Bo07ccnVdNzVUk5u7%2B1Q%2B3S%2B9vI%2BSP%2BCxPboYH3mvoUxS9jOTW%2BCmbMoX28O6nI9vYvfI5Y4oDFGwfRGLDa9852MciS5nPjHxIg%2BpJi%2Bd%2F21Z%2FdnTCp7vLGBjqkAegtjYxKeYJ0D6O2RZKh2%2FXOZCoVbKYHot%2B4%2Fb0KiRgEXnADp7dFa4pyetC5EfwMKnM%2FuizMcEJuAy2gvrGcP4w6A1A%2FMrCPTlSK3l0qjAT7EcCrKHtSpSj65WJ7UvPilBccQkKpAnOLkcvFB73lXTneG9AVvogle6xUJ54pMtq0Oip3XKzTwKh%2B1CQkBSX7ftGfQXd2HawqUYa1j%2BUy5BI%2BngFi&X-Amz-Signature=79cfa61f268614d589b0245a776d2612c43dab792fdb2b38b62e2a5b5bba7f40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

