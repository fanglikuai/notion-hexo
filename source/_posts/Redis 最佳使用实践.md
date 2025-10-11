---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RSWEFK5%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCF6ql50DNyCgEUTt9XBPbBCZXpcpP6molHT2CteCxJcgIhANTbsU767DSqI16cHNj0VavanwUZvCpp0DNklmSdtAyIKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh2CbnhTGIeqD%2BJSIq3AMR5a3BCs4pYC4GJHWCFADY1iAG%2FgrR6Px4ao%2BRx0ffdBzsu3ljBjF%2FAQspkPsRNGSEf3yEu%2F6nmtyfxqgJPFk4Q3UxjIjcFrKOI8dIjw2pgCgoML7kEiIMXriK9o%2Bd%2B0Pjena8UY0mrp%2FWRFjy0y1n6NM6%2Bj9NTi23jPwcMm7Sgboxac%2F7U66cTP6P%2BDfDu3vxIxXPJTHdotgAwo1y6VaOu8UArBh2vfHsujFenoR%2BLSwPS2OYwT%2BWAG4pd7eR0PCxHvHnQVtG0vE6Z0OapX%2BplzyD5xPejFiaKHDlOfnZV8Cap4YfWiL18oFZ4Phlnj3Dtrp55zAdvgKHpUR0J%2BsTz1ahx6Q7qijJmwa8WaHeEdBXD9eKvkIOBKYkozw70denpwep9k%2FRhB7hjLfGOOBCa%2Fv9TW3KQAl3aaphMun8fQjoDMVGLKq74gYZWhyY0A2CfTkbNcwnsf%2BcRH6mEVgWgGg705CNvOHVklsvQarXWk8hg1fPDKzMZpZlX2XlLLXKT%2FR7fCw20oA8gCMXo1Wdy9ArAPsbzqp6SiNnjW48GNpXarA3Tsywd2ZH9L63gamYOS3Ugw4q36lRHluqmAH66X6%2BN%2FKYj7Xjdro1UXnBEmJptm38RQkBxNsn%2BDCX5KbHBjqkAWnyf0kxFVz81Ll%2FvzwCPo9gd2G%2Bn229ajCHyXK5RV8jH%2F6zqC54I18GRWYQNIdzQqdhUA%2F5pfj8xN%2BbS1PznrsxItUhel4usOUZw80gGXtPA6iA6Ud%2FrfmVEoLiwLib8%2BYMIejHQKW9t0xlavdBMgsPL3FsXt%2F40uQaQ0K1V3hzf7mc2%2Fy7mk0DtuzBiQchEVJhzNBp26jJdoYTvfYe8gl5v%2FFC&X-Amz-Signature=2c577af3bb073590f52d13c0600a733af0ef9d8e6bca2d6916e248e5e9a54080&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

