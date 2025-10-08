---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466333BVHVL%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCSloj9OxPCwH2oh0K2mLgBIbkveT%2BxhOSPcZ%2FphV5ljwIgHHalf3G656pgGGZNschkR1BrvsaAqu9UrgCOJQi77rgqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFzgqEB70xR4HWfKYCrcA6NmY4jCBzTMQcPIMhp15K4HrykxDq8otq0ZYX9PuyG9oo%2B8Val0CmTSHXMZSOs4m5%2BVomWG%2BF4rrR15mNvwL1hBDKGoz1aVMzNcdw%2FztmB7LUSYklrN9iERzwQJYLBHWU5dvXvV5OLAmBKDs%2F%2B%2F8roeyLpD7d7P9gc9nAXnU0r6OEOoKbCeuz6pMMtrJ0TVsD9cD8mVweUACmr3wzaLLo6%2FBSIsgwOjZZm%2B%2Bm0ogrydkc6Np3YpXZ1kai%2Fq%2BYfl7P9q5JBETY5WAgaENh9dAqQYT1y4FJp1k%2BJ0vmYKwIkWW%2B8KsByjC4dL7vJeppsf2yAlj0vuO9gPKeQKn93Ehn8QJBLvIu40p%2FXFxQ0Dq8dasA0cp9BkODcNktsckFqCePMfIuYVy9K16OsWJM51RpTND0Y8zvEhfkGqeoTS2yrujSNpdESm3yzUmLI7FrB%2Ftup%2BUvq34oKPNQg27ejGs2qMr1EtnOnJ%2Fx0A8yXck1z760sOKX9ruVEIewCaxStFTQ45gufrRWpdNvAAEonvSLBhiWv3aQ61cPohsBsJSO6uaEMmJvJVLWHfE0shZ91Ol7hDtP78Ejuc3ehlUs2LXB2b7RQLuak%2BhrGbv1bmXzV7qZdbw4QHye%2BJ56ErMMiNmccGOqUBgxec8daB0jv9BQWuTPYrCEclY4HpzrRY9wVlRQDmW1Ibkr9Ms5Cj759%2FB5I44p6tNhT0jX0PPISvbYp4qICo7syo1b08muglPFYEwPNTKxm08tLLzs3KWXVtMpMoptWp8sL3ZYlxklNk7pDrJM%2F1shbgOX43d1%2Ba5Tc2NBHI%2FEli3gDUFORyIXbV4BPCP8Q4VeeKOe3Y8SdiUXxKGaT70HyoY3Lo&X-Amz-Signature=d880ae6e48e89013fcaecb300202ec3bf817af897b125eab6d8c28d0867f75da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

