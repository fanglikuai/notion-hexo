---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCBNXBMC%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQCUCe4mTsRGAOY%2F2LfgSaja73s1tZlgZdQJ8f8gxCh0WgIgBEQOc7Lh0Z3uUyrygONtqMGjHpNdFx5dJx%2BLA4qQIYQqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJeGDaSQEfR%2By8Lb5yrcA5yusF8nnw0Rp4h4ZXDkp0uJVsYksPyNeIjeX%2FEEvyQv1lKbL3MO%2FkhYrTHR6YFFuAShV1LB0FFmDq5IloGt2E6gfa8Dh95leWjpVGa8gKiMVg8P0ytXh1WI22MS32xWtiM%2Btup1d7wsdh3buGXVaNcK1kY4q02nsy9cgShqKMfekRz5uItYjfF9gxdOrFTsKZ0ldouW0vK3WRQTaOQ1OIJkhwTWPvJuDvwrOhMhWXn5UOsjqKBIym22Yb3n8XsZBo8OLFtdEPl8ONkECRapTCV%2BkFI0eyu1NlGdToUxHsEzS0%2FDy0%2BV7DmnphMKGJqQVk8IzggDYqJG0FcXLmDp5P53UpW7FK1dFSC5BsLcoyNpgbNs6TJu61YxkPP0A81R%2BTNP3jDUKgMxrEsLAgtPDCA9CR1Hxpsq0OLNnhZctCQkDWy6LIMpp7ZqnuBaZqKFm%2BYabluu1miy7qfqFVpKj%2Fvvm2D%2FEpDyDGSKutOI5M%2FbjrWCq8MD8JkPn%2FCifk2cnFM0SDpfxrrEEuoCPgFj043XOKYujcI9tg3eA3UFJFjbX9RBwIA11uWtPojgyDEZThGazp5jgv9uG68uEtDoCrnsfTCyFjoXLhBu4SPR0ePcK7EszgwxkQ5FfMGfMPzU6sYGOqUBwHkWzAtKiRp2rGhwgZaEYvZYnLe9fLvCuRNpr3PU%2FqNJc3JidGLfdFAC1MCm3NWAiSe8TjkwunxtJaeVCFN4wxQvzEpD2hAvQzWdOhXj0o%2BeDl9iIv9IR%2B4uiXGvtLoGZYP0Wi14zi3UzWhWk2NR%2Bk0ieTYhslWWmwKoqbgzxEL31Zp%2FqYeh2jdv%2B7M%2BIOJmGkGFvv36C37m43tEY7bEkCsDzl2L&X-Amz-Signature=995ac5f816051d30093254abf2949e74e7298a6dea0ab011f61cc3fe456f1dbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

