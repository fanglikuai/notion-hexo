---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGXQ7MCD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAUX156tzQwJmL3lJQitOHepbRE%2B4HEHr2E%2FJZ%2Fe0b0%2FAiBHSbs%2FdNNjHG7Y9IXYiVnL3xhTx5q3ApdbZPlRqkhGkyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHyNCpJzM6mqZR1GsKtwDVa5BFK0AmPAQXYeQ4sENC2Dhl7DoxmLPZVyhhhYjMCr12dMMonh5YX10F77Hc%2Fn3hxVcurQLUfTsJoyBA%2Fa4%2ByQ3c8afVmuGYAyUkadf%2B3xKPRmeBhsstCk1ArDj3uoMgMiWx%2BShPjIh3nRE9%2FOwBaY5K4EYURzqLR5ZlrlpuYuceFS4y8ZGodHMLOORe%2FIhIFHCfAOLboi38CxX5eXgPdqEG3vY4FeOtpZSH7oeD0qyYiX4sfdTULi3szbat2HQ2qEMtPzhoo6a4LilN2a6W3qmz8d5eFMCS5HphXL7ewO0d0Do4vhneBMqEq4OfEF1Kgw%2F%2FOiCKd8CkyOO%2Bznsq%2BcidixuLVI8jCJgxGeePkTDCpwbxqIsX1YOVHMzwnDVruO5d%2FqdJUXa8y%2BgBxD1HzO6A88AkNrZqR5sgd5IulVmg7YkW3TMolcEjx4hVA2qy0SZmXEnmKXvUlfH33AdWdxUe%2FT%2BXORLMh0nIeY7Ii9yKv7Sxs2SvGPnmjUgiRuxxyLtz6RpPQDe%2Fr1tZ4Jau2i%2ByqtoPeaZxgcD9Ez52%2Bdd6pOfFh8qhi1uDK%2FWe8%2BS8Ebmsls3cDmnyKhOizfsrPR22syPTvmIDRy9uS5Lj2RPjwUtn6V8ETPkedkwpv7lyAY6pgFM7Q3VCNIUTFJD%2F3IZGeQy6Ueh9xS1%2BOTouyN3F1EqQHHeWvq0qcogh6A0fs%2BWC1CV1PV2JflKRJyR1V%2FfvsQS%2FbOiO8Oa5UMeyJcWbkNwO9JCV7dJgKCPhm5FB4eurk9CuZnmQkE1Rot%2BzVSQqvZQF8briXCguoFM0JpfHsofdnplsBWXzysvg%2F96rvJwZUyDyV%2FXm7YY4izmzVwCK46rs1JWJxlk&X-Amz-Signature=e0a769005c969ba79ecb63e94dbdf62e10217adf2c9db44af1a6863cd25cbb90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

