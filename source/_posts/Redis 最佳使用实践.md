---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJMQ2IOH%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9zVn%2BEGTDkRin2GaUTOn104UxYsWX4I709Kl3dA4FHwIhALrazpRqspWOxQwzDvFfcoxQ4Ug%2BuKG5hgYMVDkfWc%2BiKv8DCHsQABoMNjM3NDIzMTgzODA1IgwZqVlJLCPbiFiVy6Aq3APvoXOb7GIw5OEllY71sokq7apv%2BtOyNzx6w9YJUBnC7CjI%2BdMk4QXcjuJbQHzbDOoyN3l18lIAgG%2BZqcciLejIy22o1bqe%2BCN%2BLc7zZ41GFPQbMSDwDaVKKZU%2FJufpLCgnAyCwi9BTGAVhnC607rmzMjggKp4Iz%2Fci%2FnBUfdL8SwTy%2FxiauYRRjz6T1oaGkgqhBh9hnbMFCccUSfWJoEeL9F9YPljS3wcAWOo2mc0qLRnHRxArVmyIf5HqCZRua7pcSM9TLwrDIjh%2BMkto3Uylc1QcR44oAz8DNOvXi0DwlY6%2BeI2A%2FymNkKfU5Q8QIrm8YZ85z5jGF7Q7d%2BT%2BMJYCM6v8SLu9ShKCTEeAuCmLUSSh7STIGRwe0WJsVWQJJUT9JOlN%2FqGoY3%2FIRm51r96%2FpA5K5euSOQFrohe5rEyawBdb57dig%2Bknj5yrNOXPHhCLZJXAvpb4bq1ZckHOYR7hP6OvBzM78%2FF37z8984zzOvNVh9Cj4ugPCgJgjI2nMfILcOssNM1BQmWmJfhmNu7r%2BxKK%2Bot%2BLlsBtmVAOewsApvSuG%2F67D1fFzasTyubv8d6CUN0Ewgnc7ejzRs1l7LvN5yd8O3O9A%2BWSjz3P51xvR7S6rkAjYWGc%2F1zEjCM6IrHBjqkAfO0VCeGUSbQ9fSJZKZlTMJ6WH3BILAp5fCfN6axASCUg2ofPLrxKBEDwkYqnbIsiDUII8R4MFmpEO5KFk4%2B%2FiVhEtlhAJdheZo1zv10SWNrZ77Bwb3frmFCLo6IMZsbFcFZIJU%2BzQv4x3Jb6e32Q1VxFIXKvfL1eTlQVhilVd4cGJIDWSGSSm8UxTh0lkrkzmmkltw5uD%2FnweaO5T3d9A0IGVzB&X-Amz-Signature=0d419853f3ab2fd894af55a1d4308dd7276943428c63a4dabc4e193e66b81636&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

