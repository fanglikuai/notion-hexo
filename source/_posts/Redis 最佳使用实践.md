---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXT7BMT4%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T000050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICpycJcN%2F0ye01Q2uZa0xGq%2FAO7dHm%2BX8Y4spTrPTbn%2BAiEAqY3RqSKP%2Bnm2Ui2oYf4oTWm99LJDxQxVna4gLyzNU2Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDMrxNiw0oAN6QNzu8yrcA9b0EiAR1F%2Bo4QEmQKULKRTJ75NkWiBjN017Y4x16oefDO8BBHSjs4YcWVCVDFG8PsSXHLW5f7%2Fd062MuAal56eRLDq%2F%2FglU4UITAKmaIcR2AqRWNsNcVazgQHMasY2QM5c82bd79LQr9tIVUSDIs5Kv4K6RgWawje41JZpH7Xsn3oxcf6ZVM2DL8viwBXT1BLUgbq1nlDqbOpWISfFyCgeZ%2F9Knr3nZW5isE6dT4wyYzp6SJGjjo1OwJEr4vA5yaAQzuPFXHFh887W1CNDjX3x2WO9WGQR3oVBL2Raj7XbeVkJEEIzdm%2BfqrzA1D9WG0dnaX0vcdVXdU22qseJMzLdVwmrvaXip7fzIWUucoKHY5psZy0wEq4pgAj6FcOxPRqE5k%2BH%2Fo5BfLvn3BXK1Yc3WqvGJqpHrJRKNGKPC7jDsT1QFzq5yMqL1JeRAychwK1gjc97qa8SZQBi7dYj1BuxiS3m2u%2FTpc7dl0T0wKAG%2B3ygUsywUIJXXSS2cmZOyjhSMK5jhevMcklbkZwJJzOEAsVr%2FLYDWsGRi6MNJ3A4jtvOWykOQxjzw9XZ%2Fng4kCBfQTo7Qb%2BDV09wdflzF%2BrudA9V2SKanno6PfdKzkHtj3cVYhGlTzHdUuJthMLXqsMcGOqUBcCD%2FyU36C9Kdv3iUqdyaRL1oXwG%2FHSt2DEu3h4%2FG2xsD%2Bov%2BEmxlnMoexL6uOy2gkTL5hYfROr11yOCTn21iqaUWeh1ZBtHOiU92NLSauckGua1Ix9E8G86HwFAR1UBxTDTpDZ8BQDpUswVsC9UnUBL8a9FNoNku7L9LCfWBzTkpkQbAtMi7uUzs5ymKmUReGuV0lzv8QAR5yD9fadnAuQwF%2FJSF&X-Amz-Signature=ed8fa8639f282e79c807baec0d312f10932832a9be0c8ffbfe4e4b77e368f2c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

