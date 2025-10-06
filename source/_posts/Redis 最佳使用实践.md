---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRBURS2X%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChsxTjNpzikR4Iw9gNoRx1UdBdGlmz1Y70HiPT3St%2FJgIgOMpoOYyB5C%2FXMKDVW6Pux%2B6dGlYscIAEJcNg9mkrt60qiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJhUmmRATyB5ZkmfYSrcA%2FvzVxZ94NcrmVjqs6htn84t7EPnIeYlPSC9294Uk%2BGFYJgf61UvRqSV7an09IYulH3oCQwpIYCSDuHzKL1xqttspaCeJWatOAI9xtIYgS9B9zV88DxrKrMcHflV2AHkNAeFNthqugGsM%2BDUn5qZZDQcBMFYjjJPI9cqtfN4CKKPy%2B1FZHvIoAbX5Yib%2BN9o5DoENy2oMQDnPwLHj3QLXYN9sXqdKQfkI83D4q1h%2Fnk3GFamu1NXt7ELoZ0UD%2BS3c7MK%2BLM4paYiy8yweKviKk09ayM2kLNQTtqxACJ7Vy34AQ16XcYBQzC86FwmrhYtCJ%2BhVqyn%2BFGYp8ke58JTQ0zW%2FksfOA49vu8JtY09rjarq3ZaUgR7Tb5w2MuQW777oqFeSMUmXTM9GNO8npxOZJ7CMKp2YTXvf2Nvf%2BTk922%2B3zYSHixXgFE5YjoelrUsNzJrh7r86ic7xlJkmO4OxxwsnFBGl%2Fui%2B68GRowzrNEUqx5%2FiJMX%2FxwmNvwD9dbabqR7ruFeVI7pDwvG6XNEniX15XGMSFFl6BYV8WichybzPP2LguTfZvw7lBHTJQtvkIHhBZZFkA2SMXVha5mGaC2kJ35ZWPi%2FcOlcAkXTK7vRrVMQCDW0T6r5Zee4MMX%2Fi8cGOqUBapsrN4N%2FaA0XNKLO7%2BlwSJUPyCbfl8purteK3%2Fx7RGT3AOtgaVNFjOf7qsaUtgwIJ5%2BIe60Q1aQ9tRZn8N1gNFUYaF8syc9iIV2gJoGaBM8SF6106Z0dg1ghFNHSJFDqdtMjTphye4IZo7c5LQSAUD3ej8I0awlaV78LnxhG3IACXp8yxPjOwmV6XRi7pexKZvnRDKC5v3aCajJJLXFEqXzbr2Vk&X-Amz-Signature=507ac8c9f65b34baa10cd3e3f9077aafdeb908b94953129fe680ef5380057875&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

