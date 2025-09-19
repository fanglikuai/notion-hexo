---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOLKRDU%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQDNZr4Ft9NheQtiNYAUTGRl%2BFcZ4AuG9h%2BWmyHTnjAbigIgfF5MmtdkubUsggHSi9J5XZZ5hk9idipuWC8jLbu6ViIqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJT9odh0Z4t41NR4uSrcA9ja%2BnnNuKvZRGejlOgr6J1GTjHnE6%2BJyw%2F4xYWZQbB%2FQYDADVZ9wbS6sUwGULKjYWVFipz9gmcKKnQVUM1iHYiWqqLi1rUZd02FwYuK%2FSJc5A8ZJy7EUwdzf%2FNsWAx80c%2FtbIwq2zokifrsRM3rHrLHaMF2SVfozR8KgMn8ykxUoX%2FC%2By0s9gNEuiXzW0we3Dp78uFBjOiILDfeMDelklVwI5L4EoZkHsSsxztcze2mcs4oBHx4AcDjfOxSqgJxS1SEN%2B8il0ayPkKxo69mbdTNbQ8mGdcp7%2FkA0rg%2FwOS5R00cKRQcXj2IBNi9MiYDjfGxgJ%2B3pBVMbA09QlvQpKSq0rG10VXRFt81sQvaSUrLOHvIQzHCB9%2FbFIdrW%2FQQifv3m%2FtHqY38fyuJOiSs6jN13zppR7yJ7kKeMhvv1Pxzje87oGmyg2MH%2B%2BOfuWmxZiaf57NnAzJGDsRiRbZcgOd8z6tQiyDPJ4b%2BS%2BQYfT3wnJnAr9%2FB%2FwwHUuNGFc9i9vXrNyJ4TL%2FQ1q1li64Lolox7bBtbe9Vu7IYKyLXXh4zVzpxrm4C6vmndzM5oQhmpfHHu5lKaB3FZj97OISW4Sf%2Bv3d%2FbDg1RlTn6tpRnP6xDP1i%2FrPOsOTDZsxOMJzjtMYGOqUBMshz0itXesrwYhB5t6UcCWzGVavRLNdV57NWtpAmzMCJ0UPDcr9SGUCGcQpD%2FuohuLmf55%2Fr%2FIlF6tBhFpM%2BwTDE%2Ft47r%2FYM1RLoMOLujcJTgBjkE7SpgYtNCsAFepQ2w2EO74llqlTKGGKV%2BxnAw%2Bx07wfy5gllYqJrVgP3dbv2I2O%2BD1fP%2FSNTUBDPGkrkY%2FKmVqjRzJj3GJwwjVePlE%2FJRBwY&X-Amz-Signature=4f2a2019993cf3239330e2f9fff33ff33d8e467c8d6070bb112b025a6a9983bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

