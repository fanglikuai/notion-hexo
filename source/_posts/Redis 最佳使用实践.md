---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664D5ENW5A%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAd3UiecPg17ZGSMjv0rwIW7GhVVghDRWgXKuZrGPwTxAiAweoIPMo4n7aEa%2B2wONWhb71%2FitEh3sKaLOiNmnxPvziqIBAjO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeo5zRH9048khddh0KtwDvvY%2F%2BuA3ibXhNPOCDFCOXEH6ouljPeARKkwENc0mEpczSw%2BxG3GMsAVMaxsgpLox7KATLb58%2BsmMxRPpBfiCFdMFVprQdj5OgLXzSvCUef%2F06QEslZJhAa3tBo%2FCNj82Z4nWOLrB3ub4%2BX04JaiYSWY9Zlqit%2FF8NoqQod3Jfvu4AfeKNb5fOVJ%2FJ%2B1gf869PYpApuJ%2BZV59H37vLq%2B2Do2STs9qTrR9R6TWfrL4YKr1orzs%2FJ7%2F8Bn5RvaZ05QGlmBeIP05wqEWJZN3qMGX29IWOSF3DmrCMwqVf0RXfj8p67yfiIxlWgs3ixtECBDGLX8wEcUo88ulMKbjRSG6AZh57IpqUTqeCtmeIM5w%2FMVd19NKsfWgwuzJMk7%2Faf%2FmT%2Fd7K3dL0LTe%2B30ExXWWIqRuc9VL0uo0WVGBpUCp%2BQCDf5HnvFiu16GIM1Ctt4z3RP5WftVMW5Rt%2FoPCKA37F0fwqZDOmhp1yKl6PhNRmVBJCqVrXvKkV3uQStM0KWhCCRToSjOiAVd8873s8X1kFDFezIHvBE9jgakh7dSuPw3yWpgTJ%2BB9Rx%2Fm%2BC5LQSjv16m6YaxXgznKKR41WFDHGvsGyNJxykWFklMnptbAXJt2fATMVEIktrpKkngw9bazxgY6pgH4lMYk%2Fj%2BsANx4N1W95G%2FYc3lRCOMGN8bAtFBZ6LswPCZIdB4pwa6gKl3XWFLDD3pa%2Ff%2BL8Xxykp3725mB3Tsf7rQVV5DjbbHJQW17Aoyx3V0RdtMnh4MXNCniAM2WYi79omzQ0KSaJho2uFHEa1J1Epc7bdgkgulGxashHki9VwjLLD22lrw7Boe2wxj3uuENS5C9mK5jKbElH1Uo9CIDOOzgWBeI&X-Amz-Signature=d7562a5c3c2396e8cd4914482df58bc5a1bbcd39843c33434400d98db19d9fbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

