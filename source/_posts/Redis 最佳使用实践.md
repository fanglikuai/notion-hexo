---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKPAF6EM%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCciAv59mcTxFk5dUv6jJKyJa6jVekr%2B721PN4iskijvgIgS1OknThCM8g79JUtNUohrhstz7U86BHsi7droaz2cfwq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDIg7Ma2s43U8dIGVrSrcA7QCQV4RlKaahyUQyJxB%2B%2B5dyZQ9QpIco9PGGjb59509tC0WD2WIAAHzkc%2BS1r1O0%2BDaAL%2B%2FNFCDn248EI5dRgVI%2Bm0EQB9nRhSYivtpC%2FPoALwFJONadeqIDykh2Y%2FQyWZtJfCzXIfRJpbqcBmUW0zGGWLWmX8zOrGZJ6oNRc7tqh4K1Pbh%2FEs%2Bjjz59QaGH4SJX%2FNRQjQqbC88qXQI1JGVnDl5s1f28zTQQY2pNj%2FsP13B6I0EyP70Hw6f%2BF05FPA0aykxaPBEhF9kzdZXrUMzF7DMHcKIBRYrt04MKdOrAOIblVBhy4PwAZFGUoj6brzII2H%2BkgqWebzf2Mnyve68ym6ARLbQ2PGV22G1jMi4QGcPANAogkifJ%2BJWhTFJSvVS0ykYG48SJsUAzVdkqEZwNOzQUmrstF5p17kcQrG%2FVp3EnfvU1sMVIlaKQVgEURi%2BLX5n34bBvsurKsRgDAF5Z91tMfyW3rHEiV8DQJprSwHA436XXKP3sxPjEPytwrsmtwpe9P3Uj%2BESZkNC2A%2F8ph%2FaMItUVPvLIUVHx6aoxKwLbrWkYCkSPHzXWW3lATgQQnai5PjoVM5gtFMokoFbUBb83T7Zr5jNgH6aVUQu03Y00e1AJ2j8REbSMKOfickGOqUBhNTx9fBerM1B0dPpQarrnYxp9ZmaJU3ufrq1cKMUGvNj14v13STOKpi6EtwwdKs2AaAKRozoBVVsaZoWJ7nLR2R4dDn%2Fubwlrds1bxFoG9uuH%2Fzph2e7PK%2F8SwTalqI8aGNHBSJnqE%2F1ysAfgE3ZyjllhG%2F7879F%2F9H5WlAwpf1h%2FDD4sNGvTkrw%2FEMUZiPQxp2NoZLl6ZA8hECS52rN7qnEjKgx&X-Amz-Signature=b297b440903a8cc8caf72b698262bdff3a36b1d72c7486abb81f47b61c1ade64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

