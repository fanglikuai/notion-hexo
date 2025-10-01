---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DYZ54ZF%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6HpvpTbC4feVVWBaQp%2F%2FZVfcerMTPrPt2k%2BSj25U1eQIgH7tx4rikaHVsxeOTdn6wGX0KgvcH0bzObxppNaQEBLQq%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDOTPMBZoYYjkl2z0XircA1VHkX92jL%2FZRk1MgmKgf2sFrI4uF10X88z3smni%2FMW%2BO%2FARH7D7XrLsdoRvPd02oN1QWz5WXLrNMVSEj6Q5MCI%2BIEa%2BHGDZx3WYHQYHRoX5C3dRRx3ejJ8vj9ygDnh9W6cnqEyVBJUUxVGfhw9IeBGTT%2FlGbME%2F8D1hAJ1jGlac4Xb5WvSBjfeUc%2FIlhRAX49vhH7JUSvprj1aLLCFNosIiMffU%2BnO07YaabBQFcnr104Jqe4CPKiXOdUdp2kRjNz8QibhhwrLW75ktioY2xxUgDNbshWvvtdEFnyONIDLXUl1R72K1uRDG4PqmlgdB1s%2BULvaa84K%2Be1cFquAKNpgyH5sUj5U%2Fb4LekbArSjAKtxjgKhajwNz9LvO3trJMockEbV2myuUrrZJEMz%2BkuUzrMFwK6sTw5lov8KDPdvl5Ak6EVlQ%2B2tr%2BAOdeZTol5ykBH694%2FUzVP15wi3Sevv6yVBsVHUXq9ui9WH189Btke2HjLN%2B%2BoLGdZcT9RxRfbLEyfRu%2B275n9WdoHBdQocoGCl%2FjOl7kQEeqneBTz6TxmG1qiuLSUAFH%2FtjQIhRMs5R8qi0odACuX0sWQEOHI7UOvxADewmMfOWXmRXrGSPZAuf7eSUi4%2F8ETTIxMKbV9sYGOqUBdsuhfwU8P8YF7HvO6N%2BtUXhg1Zr7OQtJ543j23hsnSwOkzfbNZR%2FQn2eRrKP0LTPpDYF8%2FDtQYmzMJvRGAWEbbRARvujX%2BfOj%2Fj6BsBeLBm6bXD7Hb2ZUlQbnGhXkDAasW%2BaEUkPELTZOHD5lfdpYVIB%2F8gkLCsfOzp6cjn7lPOJGLlyANdM1pCHKPFFNw1usmVx8SNqpHIv9f%2BkBM0usyvYkF9k&X-Amz-Signature=fdb3091083f586a5009830742758a56c57bdb7d72c1d8fc8639385629e6a055b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

