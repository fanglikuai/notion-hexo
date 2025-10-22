---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6V2MB4X%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQD74PDqxyw1Wqcu%2BJS6TJZxXOCdxo6PdJjtosicj6KnfwIhAJpv6Y9IzQei1tZu%2BHKjnf74wJQ0Rt%2FT1BBCxZohQTwKKv8DCCMQABoMNjM3NDIzMTgzODA1IgwwAi6dUdnc%2BShoKKkq3AP1lM7OddZGKI15M6yNJvuuMnOEZTpz7Q5VB%2BEG96yqAYNWtIQdXmsg8og8QuGwIEI5fR0Im0Cczrjx0%2BN0aVoNrTXPuVYU6rnUiw2%2B7FR33eDD%2BTOhnsIgxt9wd0xZTICaqWgCR0LmM4mTHv6r2HAUPZjazKLusJkUwywEM0rZ14WYhYN7rhwMGIR6ken7l7XAnEQF%2BPpc2thTuXbD%2FcrLXPzQTgBH59RPirExcVqWROMdXXnNeBcHqtoF9Z1tbAT61NAC64yseYVrLGqssJC07XL3zJxK%2B3rrGt7iAJBzhRZjMKFhAbBJMvXs%2BKHla3EaxdtspJK%2FusPJekRriHeCHLbgWl8StKTWi3Tcz3fJo6AYgXI%2BC4nRQz3yePGrcZbOTe5gCnEhNpIjkwsh5T3f5zXO1qzVGzlIoc9FE%2FPwAyYfAdmq0%2FjCGvJLbLOmZ2ESvWLU1py6sCWN1EkRFhjuFDw%2BsdjOKozPgrJCb7FVeihKjgFJNrIhImsxIfQKqGVh1KvWdMbfqV%2BGS6xRI%2FRmzkEMeMLZjzFi0JtbHw%2Fm%2FGK1tVGPoSculGHTHmbHmBa2kAo9pwEGgi5fpd%2F4jGihrUyKwRkNKbfa078F1as3R7llN4lt57%2FpNmwLTzDw6ODHBjqkAUcWVV9skk8vEq%2BPWe8OtT0zTqedb0ITXo5dh64%2Bphc7YS9DpoTvkpBTyl6hNbTGeYWIPtzwNPSp32twn2xzO1gO8DzeSUzy%2BSnPAVPBYfM5VJ10blYVixBL%2FRlcgnqKXgImXeC%2FtUbL6P1p7MumsHgrBH%2B7mOkP56TfTONsvSb3IcViBN1R%2BEZ8C5YzPE7dJtJXgJHfOlbrVqxuVu86%2B49Q62pB&X-Amz-Signature=ac45a9518f2e584c234aec92b0f0e00d6564bd1069decbc52564fae6d344e67c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

