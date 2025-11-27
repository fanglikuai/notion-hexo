---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JNHQ3GJ%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCxtjXUBswgNGXkHo7XPH7Xkm%2BL6GnLU7fnpN%2BlFhFwMgIgarFcXfwyYfhUGy5Px3Q6u6YonBuBu0T81eBoU%2FO67ecqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuVaFBv5%2BHaqag4YyrcA%2FIpvjGJXFoCIMMBkK5iDYXoeAH3anShNJ2zfQk8oq%2BGyj1SegVqnoZTV3Y5V%2BNqElOrI0%2FIXq7Xz%2FBcSDcvMAdzs3uzEWZIfsQeolVcXAoQfuSqkYMm7%2FZppTlKI%2FH5MfmkrWptf%2FJxFskso09t7CiuI%2Fa810fOvEeMyWFTlXRmP5uohgSLH2RDD5slqK6jG77jm8gvJv37%2FRUJCL29YWsYs7%2Bue%2FlcU51xb1nuSU6jT9mE0h%2FzwllLsUpVdedjcx7k76aW7MYrQLOCmYpaQ0quUT41JlsH5cHnWPeqh1M0x9oA6i5cAkwW2576yd0BOsC8qZirkBQSmbj8%2FI0hZVXSYvbVSIyu8wbp75aWhqpb64FzN%2B36hmF6UyFC6zCvO%2F7rnun0R5DSXX4W%2F7g9xFh%2BeuXD9Aa3NSwM2EO%2FIB8jGN%2F61nQG%2Fc%2FFqHDXZkIKjl3iBS3xAM3uP68OR4kzpk2Tovkhki0KwjiaodK0u6UXtf0wnMYskUpmnEJKXgzwOwhmPy958RzJ95KASmJPUebwlOMgJuWMIyBW9ZeNHxqa3%2F8OMhmLbkE3XiV5E1ogjLa7dY%2FJfXD4vK4ukEkruGbsRpENCk8cR8LQMdCj4XSIDlkLAtg9gEFb7p9LMKaiockGOqUBgwxvwY0F6fbUWOitRv3E3AdcBlifUoSEu%2FToEPxE2e8KUhMsRbv7oxZya%2FJ7fj3es8C8Ca3fqAJSaDI%2Fdzp4RfaCs8jG64w9g7F4t026Iwuva1oiKSytNOpsVUxZlkrVsuCCqq5dQq9%2FRjzQM3uQnNVpzcD4gky9NPJWqX3%2FzmFX2tkDNt2T9geWioXPFGR3hkKgOcTAUSx7yDO9qWHDCWTzHvdA&X-Amz-Signature=10d745df53d775631dea3712a3f3dc3cebd20e1ab7788d743956673c56f7e216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

