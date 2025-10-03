---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4UUP72M%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC73I9fZX2sheGlrPtPMCUlgMo6%2BYQ%2BW8qXKFjlGFMVjQIhAJpzG2zeIeVnktlTLjvm833cgDX2MPoJBK6PfjNihdVOKv8DCD4QABoMNjM3NDIzMTgzODA1IgwQLmCbXtnRpJfDXMQq3AM4%2B4V77jgTtnQvQZZbnuVC1HdA9lmMBzyZFKy1fhZitb4zbclsCc6uNyFk6X51cHF98tyEzTfhqEsBNsl2GZdZ5DT22HpmS5bjrCL8nAZFiqHkwtgxqdXv8c7ic2rcWWfrlwdoyS%2BhVHbgAGdosJ29wgxkDb4rslgWlkNtb61C6HPnIZlIH9dPNI7IIjNcfNHGDnteCln6qsNLgKkZOhpuLurqY6sqFYspo%2Fm5ajljn%2BJVSIq7nVKQQ7tebPQjLx4cl6IY4%2FrJ2lOSLnGfwdDHeyxHoQdaLtjvv537j4RXPfDpQop5NwrsPkw6Hfu3NOSSa2o9PAJw72IAxAFy8sFsREJ49tWBpmaXEQO3Tu1Z6Heewfu2%2F2ReP8KY62cgVr4pDkGjiRHD5R9OsjtSDMMLMn%2FoJgK5EjAl4SqMxv8ynQOWwxJu5wd8rI0zm9p9RC1DUNpU6FIQHbC44TKZ%2BcP9lKMXgMyzPk1Gy%2FGUkgwglLSY7z6YKKeXYFHMZKl1uoUcwIXPRNOucF7lcDSl0uVk2jZIVdrip6EQLBUpQxGTbND8rqN2XPRe9gyqUuHRKCi00QclKvrMUgLJcWfgPSJ8DJeljiPYOljgnx7WaoCltApB3U82K2RV8jnQGTDXq%2F3GBjqkAeTVtQSX7Rs0vKQzfDfWRwhYUCTN4suP4zzDEfijVxgbEcK6bQ9JrKXHEczVEtn2%2BJoeScw%2BCjl9619gxL8%2BpmS70VBs4aDF9sx0Hpq91kv0K4uPIgwy6FjG%2BmI1wEoyiK7MMxGo4oyitl7xLLZOVzfg%2BBLvaO4Fr8mJDMv7P963rlxKjh%2B5y11tRf1fVYqiBxHnag5j57onGBqRKkEty0KsBk%2Fg&X-Amz-Signature=57dde2eabc08a74ae8c92b87d116fe6ce94ee4e7c7be2aca82c9e5a189da1659&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

