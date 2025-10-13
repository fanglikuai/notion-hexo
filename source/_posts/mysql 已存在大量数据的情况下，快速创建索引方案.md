---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664H5DZRHN%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T140108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDsUe4Wq5L3KNseRALcl8mx7%2B6GAroBVRtETM98x9%2B8BAIgWM03u9k0fObZveUprtRiCQvFvXpI7ZrQetHQZcCzBZ8q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDJsRuApzec8547zWPCrcA3%2Bj%2BenySulUsaLvSkmT29IVJ5OEI8MWMr9csG99g91CW8cXW1zsuRjMB8izUvkqHL9Bg2boJR7PNQ9b8yg05q3RBfbl3QkmT8v5s82F%2Bd02QvOj1UJHc1IQm3cW%2F05Kl1r0lVQFxWq3Rb4jTBzd%2FsF6dRvFORgTJ3x%2BaDxXOO8cMvR9AblAMF%2FmDAE44f8xIYuod2C7VU8IrIdQXC8EcQtltwPoL7n2cXRSPOou2EmXG%2F%2BAb2556doH5qA9LLt6yDWa0UmGWn4hlIpGW8hRD%2BPDQLPeMmtbEZnSm6p6dBF6GU1g94gGFQpcO%2BDLqe7maixvbZHTwXh0%2FZ8K8vmlIpxdkHmyecAa32k6IPzcU7fYiJxQYccXpck8KYNEKKtWjHwyTMEHycE2oTVj26J29gTo6blJVY%2Bx0YgojqWOPOI9feefd4TgZlUvDk%2FwJPodZgua4ZFfEAerz8E3UGCWDgMDERra8huH4fONjprTg8KvnuWpVg5UvM7Yjc0FGCkAFH5gqoUob%2FHtZzQ%2FeMWjj2EhXMsBv7lyA4IMLa%2FdIIMhLLDm8t2RGfABzMI0L6Lu4aO9mJ%2FgWX13GVQJdqp1HFhuNuzgtdPxXnOBPxQsgOF8nRyqd7VdHPbxr%2FcUMJDvs8cGOqUBjEoby3Z5da0syqbEu%2FsYZ09%2B2SpLwgBH10YRUYFWJi6%2BOPGo6Ggz1MLbDZ7GnyS6GbwdlEbFc8qo0I4P07iSD9BwJt%2FFaLXN9m1jiaixxbgRWgq%2Bx9o0SzUCm9teD%2Fmjd0Dn55%2BWA3e5DOmYeRjheRqm8oXQK2JS3i9H29kNLF8PCm0nq4gCwO9RsqN6jVaY40a2YVKAwH2iDQtpKMWUQQpDgJ5s&X-Amz-Signature=44fbdabfc21d6fffd19d819d18b934599dcdc5b1aa0826c1cd5e874bdf847887&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

