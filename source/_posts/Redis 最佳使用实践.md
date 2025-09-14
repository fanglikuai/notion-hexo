---
categories: ''
tags: []
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 17:39:00'
updated: '2025-09-14 17:45:00'
---

# bigkey 问题


![imagesc6758344cbe13f3ebf0f8718f40ab3f3.png](/images/2b5b9358f31c71aab0e0ed392d72ad7b.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h <font style="color:rgb(48, 49, 51);">10.66.64.84</font> -p <font style="color:rgb(48, 49, 51);">10229</font> -a xxxx --bigkeys`
- rdb 文件扫描
    - 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key
