---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZRUSCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T220113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICcyaYecxAEBNnyBy738c0Je4e8rQ%2BrbOe2IXEkvmexvAiEAv3GJecZH4C%2Bbz8zP42aaIhkUFPJCMjewi60Ua8G5Obwq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDDCsqYNVvafZxLkRDSrcA2vBaxZFaoHa4bWAUOFAfe%2BE94CjJy8oy6UATHxD15TFiCs1hG%2FkDyhFMGxQOEk1KBSlTMdiyQyiZ6A0wMClRSW8zjVCK1GygXjLrciOe68LO9L2Fx09vmmWenmjf1B2HqBaUziPJsSkZiSFHHCRyjMuSsRRPl2MCRu1Yqw4W1LfNScn5RBdk91sjoLzSpIoR%2FWIwmkjleqYXLNSRvEqbasp9KsqIc6fZ0acBgBLkNIH7MJbppy9SkUFRvQIFZ2B2qhCHDYy4hYJNVMqGzgMI1LYk11MiNXGwvJyJ9pGyE5RGH3rlcxzMKjX1uq7fMDQFHA88OWT5cIWrkQ%2BpReLXNTyX%2BtlaeElG9neo%2BRh3cq0G0C4%2BBlNFWTgTk9CtroNW2DjUW%2Br7BxkZ5fysoCEzJG6R%2FLUy20u50R8PgLjLpiTEmnKURMpi%2BtrGSwnPQIjy4r3T7LruvPZAHD9qGOdlzSiyiGG39Y1SMSGD6B%2FdlkgqzD0PnR1YR3br8IHX1xdUnnBIGx9NY%2FQUA3jft%2Fd2riHMHCrL7kufOjd7pa6uXS%2BPdYoEbkucxfMvjW7RXhaZLElKS18Y4kTLWql715Wt1iuTNeyTqBF1fVDkXamIt%2FMIHPU%2Bi47bIzOxoO%2BMLuUk8kGOqUB9fNc8ZXZjQMFEdXb2hYcWxCpQVNtH7pAVYxBdwi77kyRp0oDS0%2Fj6CtgLepCPnKOZMyPf5XHS8ZIt7AJxJfPODRfITNXfS4jPsxK97HwB9dCqmlfymusThjGzzyAYU0C41mNfIOZZ6EijY%2FdzdIJ0CbzfBABQAZAJJk8ppjcqsRWqpXxg6UbnxEvbqvd8y5CrqEFs5GFQp02eokrXIuW5j1FKA17&X-Amz-Signature=4affdd684061bb3e347ada7b62b905ba66ebc3cce4a9aa2c5b9f20319b9aff2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

