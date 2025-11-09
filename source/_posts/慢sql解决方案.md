---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBJPVR46%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIGYb%2BslfyaAFKs9MJYV9fsMykDmV%2BgGZqt3WmwwtvflnAiEAjSUBfFvcsggOgZvYSoJgvIsZPWhdLwAmDuEov95eKxsqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDARIeesTFxeu3icyXCrcA%2F7azl4XNJRZanx522pBD4JpvGbUMk6Z4PNP6zH%2BpSeqq25hQ41OfBs5O%2BAXuJG7%2Bofdme2JlBBMZIqgBtxjPl4nXawj%2F15aAx0iBnOL%2F5C7I9HAUf4S3HzjjmNad0MwBffAfzWWu4I4PgLZx4YfgmV7s2mmFI4s0YokitnNEu1wqTvDGt711cZTAfrQaEd0rW0LrIBHrHo%2F5slTs2c%2BVzMM3fOoRUS0BstKUAi4esj5vCjJp%2FErLMIK41X%2BHJkb5H8n9CCP4x8aEi%2BWPOjpazqW1n2w5FiYd0ahRomgKuCAzxSN9%2Bw6IyGMVbN3J6EVMxjybxkAPMcgTadwzYvhUlV%2FifVpBoFhHOl0uinHF3GpufdkIjDfWREspKOc0Mq9Z7mgxJU16c71pjpcBKAUKasjvc1hPWYOd6qNqHNKEg0LLdkkyhT5pqWmstV2Uktl4Ocn9aEgAgq4ssbVEs5SkQXDYMGNfN4B90O5rLPmynlXuDR6sLkQlUO7RG75a0q2H9F0xQ3x1lUB9Y6JDSefjOHeU%2BXoZW016ryhFkIqfq20JmBoB2V4jxq7CG8PAbUhJ8QROF3UDrlB5cHOQvVmtiPCGO9qZbnKY%2BPny1JQy0NkYUgtKAwrmwqD1gqvMOeNwcgGOqUBn4xm%2F2%2BzqqNR%2FGwS86lE%2FTVGQ75%2BlHXcbISg9YeA9BrZz6KuERupSJA0vK9%2FoOVStcictvJe1s0rtjhg1rCDAWzWwfQihr4%2F9LcEgKeGVd4hzd2oX8I6RoQ%2FGKcDAL%2Bk8NBX8qPgZt8TKY%2FA5Mm9qVdLpTDnpB8JN4CTwr%2FDm9U4XZVA5kBW9BiR9GEYtQMidhprn8f2XfHP5qqlew%2BMNajca8Xf&X-Amz-Signature=0e4f3eb3051d66fc0954a7ab7461bc5ba2a48c9521c73c743d173f9c5ed5b437&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

