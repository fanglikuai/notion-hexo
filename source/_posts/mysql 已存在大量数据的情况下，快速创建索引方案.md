---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ECY2HUW%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ2OQaU8izDq9rvbLZP7c4JNOG7eYRgLDO28VgYtz0kAIgTLE17EyIbAJzNEEIf7y%2FznANpGoL8PzKKZGdEy1sSmIqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ6UoxUsqvvoV5b7XyrcA4%2FypfAkjfE2WBnvi%2FQKlzBCHVLmESGi2pbJEvsiBEGZR%2FqWMVx6nOoD7zbL6MDm7VGCztkSQVjvkWPhUnIrgDbwrS8NtJFJz2TyJk%2B%2BWW6S%2Fu1WA%2FOGPgeVbfxUUErS6YeDQUR6Ai5TNtcvPljH6JVnJ8iSqVnX%2FUQ7nRQfW2vv32uHDKIHo%2BewKBPfKwSMaT1ZnV7Wip7XGhTGJV2WsZoPR2lfiQb9BZLQw2S2b69PrS3%2FdWRggeCwjMHKCOeMCOVAa97zIKG0SJ2QFpmrhXFGPyWGqukCWLp9qAUTo1NFElD9OeMITSMZ2cl4VzYlGVhsRNEqcDqM%2B9MzXA0yt6ITeFoj%2Fx5XoNOPqCOb5%2B7qUzunEN97UjHaSq5YOJBSpOOAKNT%2FtdVwvR8Kv67Uq7h3PXVlQbyRi8ie%2F4CK2YOkbA4Dtk6RDj6u0WolZZhLlvurEqlPpxbAwxnhK9wlFTSQkfvo4%2BN94VhoowuLPs%2BMjc0Wq5J6Y%2BeWOpNDWpHGIMNCpuNr%2BlCbge%2FIGyje0WMRs70VcQ7GW9rEEETgM8Tusm7vNx7%2BEIq9%2FUCbIKqONMlQq4Glza2c2hEgvhRNJq8NynHEmy69UnNPGgQkADKdDhRilJc0otnchTXBMLC62MYGOqUBSOKGxsPu9kDDAibp7lw8JEmdo%2FnnusfTl%2BIlxdoEJWK7i%2B0KrpxqPNLhELLFYalA3gBXjqtCe6rmMuS%2BA5LRHxoYS9mhXIGqWoImQR9WIJ6REpFysiSJy82pwSzkMIgezK1wk%2BST7%2FtmUr0B3UEP6rQv%2BuIFBhVf7%2FAvFycHHYrlLH1MrtCuC9vumzjDCOAut%2BuQ7BZ4aSQJ8Ottwu%2BVpGDDsKVp&X-Amz-Signature=a28d24d93d5483682f7fce2386e7cfd0e37e0f11c8935895dd9380ea9cca8f10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

