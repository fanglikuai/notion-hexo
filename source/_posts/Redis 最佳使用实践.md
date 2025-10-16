---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663W6UN4MR%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkwkmukkU3Axafh2b8HPMdj%2Fmx80TC841vSS8BkJ5izwIhAJoqRii7HbUsydNE%2FansFMoJl41TY0E3XXTyuJRTThZLKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxDGbZ4D9ISt3KqFWcq3ANB8HZOckrUmrxicAOqJNhjSdiMIeMwghK5oTOjmZKrWCqIXhp8h9ffKBlUgnAOL5730aIJxVSd1IP1mXvrL8sEKIXl1jog3Kj2N4se8vtvzvZA4WrVyg1TrYy%2FPoOGjUJUU%2FyaNxkO2E8Q2sQIGBFQWOjZNnjN7rhTXiK8C4SBU8%2BlXHUxZfOTHFtGaPCEkUM%2BcFwPK8zWsZ4iG1JQmIEW549QbpIDAP96v%2FXI45kxnrk9uC2O37BbiR81p7Cmz5Ks3tHFNEbpG2BZFRrD97cOun0qEm8rARx%2Fk2tuXGlTGHGqnlm2U8KArMv1VWYZ7IO%2BS%2F2NRUux9cYGcf%2FzLAP9zy9TpQzIXyVYX%2FxKnHZVasFp37X7u2gGJMNt7gSNsbCr10opXsDNArMkzfEJLBM%2FgbKff8nJsPCMG525uR4fgNM1KVlMA7Gd7GwVlzjcFxRemleMX7F8LDQVopLEsv6Ene2laXpfH%2F1zVBsE99jodSWccTTnRoHciXrrkcujlYhjcIowTYxXeRe1litU6%2FzS%2BwL5A38Zj3%2FaynH266LIwR4l6x8WmLVyczU8dOuIYhRo7stdiGGWGuCAobTwVTX7eRl2KI508BeWQ0vvfJ2faUyfwNRCOxNj0mg8zjCGjsLHBjqkAdVJgOtt9K5T0wigEmQu9av7u2mvorGNkrscig6UcnQymNa5Qgacz%2BxW7svVq%2FwcYZ%2F6uCJtMwRiLRoR3YD3KddI4mptBBE%2BWSdq5RcbmHzqLrFwlXxv4LbMiIHottsIrb7eaLjzvZrqW0MCUNapDr1lxBrzvb5waYcpNkw%2B5a2Z84yBjeBlzvvZNaKQr0Rps4d6dJ5dLAVdDzAd195oR%2BPLYlf1&X-Amz-Signature=9ed5a5a99ecc2432c4727170656154c9e4e72b4eff6a8c40c15fc0c66f706f39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

