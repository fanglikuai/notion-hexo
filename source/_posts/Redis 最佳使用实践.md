---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646ZWLPAY%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE3DwxT4UbApUYJiEatmbp8%2FOgr%2Fad%2FIdtrie%2F9pYocyAiBJr%2FxA%2FCcGZkBVNzHI4KDIyrjRC4ep9SopHpYKEFBI4ir%2FAwh%2BEAAaDDYzNzQyMzE4MzgwNSIMp1yDxyukphkluEbTKtwDyJMbD%2BDEiGeTZWLXkaM2B91RmTQ8qOiMN660m8sZJMj2%2FgPQjvUS1Q9gthwujRWMTZ%2FOvqzPK9nrks8L2vz5TupvVZABXHrIdgrgr5oeVupUkyXceWoGmzyvHWrkgDQlLgU7GKXS%2BGB3nqA%2B%2Fo6899OR7J3e6DUsGRgyB2X9AQ%2BDZVZGmcdSKl4inVc%2B5ufgJ1%2Bmn7xvW2p0of8CUTyyZLUjXGGZsEZ2wVzHi%2Frc0i09wYL18cGjEwK8TNcAKNZGmwz1bpKeOZ0bwG2VVmgiHmT6C5j%2F1pHeQ01XpdfeFKFdJwwtQVAwpGfNLHKkyTZ55irv9nCGN4Rrmd%2BkBOjHy8DIxEX%2F79F8UiXlEFQAs1zyf5k5Pj1MfQ9BY0FymgkIpJikGMxBNuAVWPevS6XCBqmtUc9DQhBIhNWNaRWaoEU%2FiUVDcrAMCkxUaubOxs13ucBGNA6qtLNvSI81XVvZ6vbW%2BqreQGqlqLVN3ZPLQatqbD93t5PMPPdcmuAoaWrHZFEowF8AcULvjr8ylEAPrREmWNv96nAt%2BF7sOZOG6lgavbWH06zpUCSWmJH9RlR6v8SBb2J%2Fgc%2By1FlGeUta%2BBRLdGz3tb%2Fr1lWunzf5pStdjjp4YGIKwEOP6Oow9o%2FAxwY6pgHtpMIarkmOHX3OtCq3SydMgmqXVAmdfgwMLqqgjQKlu0BNztEEYTizZjp4MkiOvWej4LeynXNUGiDp%2Bp5VV5dENDFJ%2BjIYPMWG4eYb0vXcut%2BkEaHUjSvjmYRL5FM9AZuFamuSzs72Diy8BeVbin8PmbJUsZcwY7YRc22aV91aTykcmS6Jsq3xc3xASjzWL%2FogRTyShLgsQiLmNB50SFXGjYNgoTjK&X-Amz-Signature=350cd3db3626c135fb0856d0aecbd13c39be23a0784c8a4f06b94b91f8dbd8f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

