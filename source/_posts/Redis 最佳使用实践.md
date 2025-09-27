---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYHSHUMQ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCITCdOkvOlDKnQ1riifNL4sG3B3%2Fgr1ilD4kyPQV50IQIhALoGYNJeXjZHS%2FweaaJRWTjO9cy%2Bn2YyYcAML18oNqKsKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FtGLW1R06ZbnD%2BYcq3ANr5ABLfWNgLtekHVBCgKMmFKwMHA4pD2kEggwdMm6OQbJ%2FgwBez2b6I54jzA11tu3%2BlRuDpVfCxeK2ExZPX%2BMomBKuPjp6tEi1vrdfa1pIcxsx0zKROW30YVck3hI3il7vpbrx%2FElnMCQhDc%2FtXuhK%2BIZavLW3%2FU6q48wB7dARjKbo9GbycJSAK8eX9aF6djWV0Gi31dbWtP7%2F6TszhQNS%2Fi5WfC6In5%2B5h5gOGVNbkOZIl8xzUCZUtd7ArGOElrHq8gofxBHPd%2FmV5gt%2FQkwCMhfDHrli6I9DeLsZgvUOMVzx4W%2FY1McCpqlUIWBuhAM4EQ1lCeM6qixiErp7cbBqYsjea6DwsmeTIgJFdKoSj6FvRhrSuWeWw4S5q0unqyp%2FLvMb0OXZp9UKi2YOEpsyISX2LPl%2FjFyvnhLY5xp%2FqNncKk1Sk66VVnQEIGo2Fe7bePg1fibpW%2BUNqE4AryZwOhi6hzwS87ZUBeB7lWKvZaZz2gfybtAZ%2F%2Bv51RmEUZBg1iFMPYFg6kqjpxLf2w1Ko8vbJ4g3rgZePcCkecZXbxxiqzgiaeYXlAGyiqlEZ5KPsUgdw%2Fr1msEB%2FJH8pUrcJ62hF%2FldcQEA6f9nksrR2uqRYtzefGKYy2XB0jDT4t7GBjqkAZ841ma4rGE0c%2BrGXxUsqgI0wUQ%2FUDrcXtT57JnHM1rSOTVX77tPLmaBQjKAm50VfuKSJH9ACsBnK%2F3nkgYDGJjHGFtYFdnsQ0wIcLwiSLygi1SEvzPivUrtdhKUCsDXC48TogwiAc0%2FF6VWOrAmHDqmGz134xyi7O0zTPO6Lh%2FGil1TPOV%2BDH%2FkgzRFIOMLe7Cti%2Brwxl8%2FHZZGJR5nM9eYWLb4&X-Amz-Signature=7b65fbe204e4ccd68119534ddb4ffd04d9a443bd21aead335c41a99314a39c9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

