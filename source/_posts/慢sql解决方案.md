---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664H5DZRHN%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T140108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDsUe4Wq5L3KNseRALcl8mx7%2B6GAroBVRtETM98x9%2B8BAIgWM03u9k0fObZveUprtRiCQvFvXpI7ZrQetHQZcCzBZ8q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDJsRuApzec8547zWPCrcA3%2Bj%2BenySulUsaLvSkmT29IVJ5OEI8MWMr9csG99g91CW8cXW1zsuRjMB8izUvkqHL9Bg2boJR7PNQ9b8yg05q3RBfbl3QkmT8v5s82F%2Bd02QvOj1UJHc1IQm3cW%2F05Kl1r0lVQFxWq3Rb4jTBzd%2FsF6dRvFORgTJ3x%2BaDxXOO8cMvR9AblAMF%2FmDAE44f8xIYuod2C7VU8IrIdQXC8EcQtltwPoL7n2cXRSPOou2EmXG%2F%2BAb2556doH5qA9LLt6yDWa0UmGWn4hlIpGW8hRD%2BPDQLPeMmtbEZnSm6p6dBF6GU1g94gGFQpcO%2BDLqe7maixvbZHTwXh0%2FZ8K8vmlIpxdkHmyecAa32k6IPzcU7fYiJxQYccXpck8KYNEKKtWjHwyTMEHycE2oTVj26J29gTo6blJVY%2Bx0YgojqWOPOI9feefd4TgZlUvDk%2FwJPodZgua4ZFfEAerz8E3UGCWDgMDERra8huH4fONjprTg8KvnuWpVg5UvM7Yjc0FGCkAFH5gqoUob%2FHtZzQ%2FeMWjj2EhXMsBv7lyA4IMLa%2FdIIMhLLDm8t2RGfABzMI0L6Lu4aO9mJ%2FgWX13GVQJdqp1HFhuNuzgtdPxXnOBPxQsgOF8nRyqd7VdHPbxr%2FcUMJDvs8cGOqUBjEoby3Z5da0syqbEu%2FsYZ09%2B2SpLwgBH10YRUYFWJi6%2BOPGo6Ggz1MLbDZ7GnyS6GbwdlEbFc8qo0I4P07iSD9BwJt%2FFaLXN9m1jiaixxbgRWgq%2Bx9o0SzUCm9teD%2Fmjd0Dn55%2BWA3e5DOmYeRjheRqm8oXQK2JS3i9H29kNLF8PCm0nq4gCwO9RsqN6jVaY40a2YVKAwH2iDQtpKMWUQQpDgJ5s&X-Amz-Signature=06ae9f2f09bc48dd16510d5af558a9493056b28076eb7b8dd6fdfb271495591c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

