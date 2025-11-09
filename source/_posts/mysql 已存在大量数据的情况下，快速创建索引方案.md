---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBJPVR46%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIGYb%2BslfyaAFKs9MJYV9fsMykDmV%2BgGZqt3WmwwtvflnAiEAjSUBfFvcsggOgZvYSoJgvIsZPWhdLwAmDuEov95eKxsqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDARIeesTFxeu3icyXCrcA%2F7azl4XNJRZanx522pBD4JpvGbUMk6Z4PNP6zH%2BpSeqq25hQ41OfBs5O%2BAXuJG7%2Bofdme2JlBBMZIqgBtxjPl4nXawj%2F15aAx0iBnOL%2F5C7I9HAUf4S3HzjjmNad0MwBffAfzWWu4I4PgLZx4YfgmV7s2mmFI4s0YokitnNEu1wqTvDGt711cZTAfrQaEd0rW0LrIBHrHo%2F5slTs2c%2BVzMM3fOoRUS0BstKUAi4esj5vCjJp%2FErLMIK41X%2BHJkb5H8n9CCP4x8aEi%2BWPOjpazqW1n2w5FiYd0ahRomgKuCAzxSN9%2Bw6IyGMVbN3J6EVMxjybxkAPMcgTadwzYvhUlV%2FifVpBoFhHOl0uinHF3GpufdkIjDfWREspKOc0Mq9Z7mgxJU16c71pjpcBKAUKasjvc1hPWYOd6qNqHNKEg0LLdkkyhT5pqWmstV2Uktl4Ocn9aEgAgq4ssbVEs5SkQXDYMGNfN4B90O5rLPmynlXuDR6sLkQlUO7RG75a0q2H9F0xQ3x1lUB9Y6JDSefjOHeU%2BXoZW016ryhFkIqfq20JmBoB2V4jxq7CG8PAbUhJ8QROF3UDrlB5cHOQvVmtiPCGO9qZbnKY%2BPny1JQy0NkYUgtKAwrmwqD1gqvMOeNwcgGOqUBn4xm%2F2%2BzqqNR%2FGwS86lE%2FTVGQ75%2BlHXcbISg9YeA9BrZz6KuERupSJA0vK9%2FoOVStcictvJe1s0rtjhg1rCDAWzWwfQihr4%2F9LcEgKeGVd4hzd2oX8I6RoQ%2FGKcDAL%2Bk8NBX8qPgZt8TKY%2FA5Mm9qVdLpTDnpB8JN4CTwr%2FDm9U4XZVA5kBW9BiR9GEYtQMidhprn8f2XfHP5qqlew%2BMNajca8Xf&X-Amz-Signature=5dd5f711f4bc9b7cf3e0ca004522503420cace3095231c1f71ff7e25a26a545e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

