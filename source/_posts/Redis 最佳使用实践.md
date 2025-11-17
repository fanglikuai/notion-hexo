---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNSEF5J%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA9WfXara5A6LmqfthNyKy%2F7jvkIItipM5%2FsOXI6xa8jAiEA5K2WnQttw7PrNLhou8OR3zE%2FL3cD4ZLIAyYIsQt7DzYqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKyl56YneTmGGJfLircA02PNFJiF6ygNoyvVwPKbihx5C%2F4Xw6Wn2nzrb1Gz0eBpjRrl7JHE0WBl%2BmulzLWyIky02HUnh%2FR0nBYqhW9ix6lwYF3RMArKPDr4QA1V17miljRm8SSAlI2c2FEe0vDIngWKs3WQqZanxI2cWKois9OY9iU2ij5dCMBkZRpXrGJp4c8TI%2FnuA52CYD74wIF9SIYed05G7uS3ASTDrVVsOGzjQD2SVecA58KH1yNtNiK0BZF5d4tNxYuQYWJ0TCoRc6z6WmLZwjmYLgGszLuz5TT7Hv4a8vvfSO9ZToY0NoIdviF2%2FYyKGBf2czXnT1Wnt%2BU6vLSPow6N5fcK5OiC%2FT%2F8zZqbJQ1%2BuLD9u7K7bdJztGP4%2B0MNHJaKHDNHNwcCBijFmjnYDoFq2UGpenjnGjEbt1PrciwRQRbiOaPWvuY58ZTfrrEdhWow3%2B%2BHBgCud4JEotjI%2FPkCuKIl2siJ4tdeRQxyll9ZgLvvy4LUgE5lL1I6hQLCELIRvd0w40Zt66SS%2BlS%2FAziAG%2FPEvbqwE%2FaVjl2ZHqblczNVH7iRwTfLOGrVErY7YchSJ2T%2FSu0zvCCYWalAJ2PKC6RDmh68lrYAlobdlwoPy%2BKIVXEJRyesqSYYn1YEpSLaaSkMMCl7cgGOqUBCZYZOXBDJUDIW%2F8rnLq1M47p21diZyih0YesImLJm51WR5%2BCwc8QXtLt0352YxcZtJuizONuoFQcTzr1U91aJDImk9%2B5jrYsrSXqblTPLxZokR4dz4IIiZ6xXbscJZm0qJ3x3LLaN2AiB39eUvYhN1znYG8jQLUUHAfnv0HS06CL4x2Tz8%2FvVTrmy1VRUMSIclxbngdOaAREbBy6k58F9e%2FskwaS&X-Amz-Signature=d3ce67ee0479caa8d5c569ea4bc5084625ed8e586e9b85ca3eea238661bfcfea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

