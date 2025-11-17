---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHVTM33%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCPtx%2BlD5vWqEBdU06vUuIF07iuZIqefR9eBK8pCJ2ZAiEArO4NzoeuN96oIDchm9HgV6%2FiDv8f18IMOOt3k5MeXGQqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPuxRTtEnlGU7O5mKCrcA4VouSnBofQCgVwisVQpxF3TCfbKzT6u3AGmJthZRoOvcCuii%2BqHLoDWH4CtImL0kewSkNkOBy2we3IRdILoX729eCurjzVXsJNjdmSBxNmlT9Wwfa28JwfpXSY7BIHWuXVJikbC8deaEiMLDOwmwhMreopOQQR0zd6pAKSiYUs84z8r4BcwEf4N6kTAq5KCwkjkF347q%2BlOFs8L5MOhDbFdjdPXCjqR4rr7yIY%2FVk9sDv0V1CpiRyWZr7o1PMjnsvonyg9WUQYfLvnT6DOvgH%2Bf5deZ0Fkl0wzg3pIGdwkqyPvnswANtbIj%2FWKaeqGG6iNOyei5dnBYE36CGk5MXLwHrVU%2FG31GHe7TACocqd4iyGrs1JNX07Lq4i%2FmyucMIn%2FJ72hJl9V6cJVW2U1gbcW9cH9mkQhD94kXJTjEUT3TXWZ2puVNlY%2F3F2tAMOfLPnXP4Agae7wahN3YEQriJE2qOc8fHpLeVmJfW0yuO380DMLm%2BxjFgaYR%2FAHvAG3LDj8NMGE98luFisVlHO1JxqjvHeuotXgQf6ZturBm8PbLONaa3PkU7NH1%2F3IL5qoiVJSO9BCpsAC6bRhMZ5oZwvyZMyZB8H%2Bqp0PnXkOVT%2FoZsfLyvytU6cCemBNGMKnW68gGOqUB32IkhvC8iQDnQ9dS9G7vYXrC1r%2B8P8wc4nCXRwgf5GVOPn5YPlzitdjhUQEcxhfKsxywRz3zJT9LDlzMDb2U%2Fi0cYlKSrb4yQkJGBK%2BD2NA3j4ufQNTUpTbxlBAJNVEYtel8KZEFf5aUmR1gSZ8%2FqIKOdyksuSE%2BpghuIwBfA6OlV5YPmZ7e3WjkkqMAlayxitPa3GvZsbtBc1L1UzuP4CzQQNBQ&X-Amz-Signature=a25e8b8735b0114d2a087964f00fca41aeef2d76e9822a84aacd65c351f28c47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

