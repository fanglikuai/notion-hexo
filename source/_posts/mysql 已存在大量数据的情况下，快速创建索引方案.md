---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667JEW2566%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCG%2FKagh2jm%2F%2FookKHaXPTyMDWodSj9tP%2FYbQ%2BDnKvYJAIhAORH%2FlSGN6KRw6GHWuoafpRdCCe%2BrqqYJ72Ap0Pqcr6gKv8DCGEQABoMNjM3NDIzMTgzODA1Igwv5hpx91cshR1cZZoq3APo7pnBXZe3HQzpbUFs8y4mIso9ISqgq44vz%2F2UuXL1jD1tvtCYTVXVdVbT83gB%2FR5Aufc0wKQGM42rizov5iRWfvlh2uYZPrqDgQfVgZtppsQF9qDBDw44ZQDKxX9uCDmqwB2lPRSJJo4nnFTaXypFo0GLG0HTV8Ih3cGPaTPAh1pp3Evp%2FPManqlWh4Zc8d9A0jYxDKIXDUFcGs7LsnIaaz0L6g9c0zXU1n39zFYUVvztJ8oDIcZsaipZB2O32VI64Di8ArTtsmBb1qgmZFUEhYoyhOxo%2FCxoUKnOJNRs1NYE6BNGwygt1Br%2BKAq4CGRJpKTEwyCwbI%2B19d2Xuq%2FLMiYcUL6dN0v1k0hrt3Air%2FqFv4OPJ2XSe7VpQLhuUUkvlnzI3AYPflkmzPaiyXyG8sKigMFL6OkSCo8hFlxdAFQxu326aMHnd0MraIkHWkCwOS2RMx7ayONkuK3UvdpZliDXts%2BHbRws6tBkLG2zTe2TC9DEIeDyMi8%2BTLV%2FzuTAfH1iZwHFWai%2BVoNyvGL%2BIdBWQJQjAfFpqj%2B1K6bUrylayWmDQ7Es730aU5nIGdGKP10woG5gGGOdrU6eRU3z8P9VDJ0Uo3XphjtNV09kOhRByul8FqjxyQcfTDCIwdvIBjqkAc2uoER%2FEJd49sDb7zJiIFWktxYfgLMJXf1YwcuDTWeFGUFAJN2CiPNrEwKYLsRxkzqPqTQySLJ8cPCSNf1fWSmNAneyTAI%2BIbTrIIaPg4J%2FLuQyiO3vzE6sfsQ9CRl24m8s9K54K8QZT4SJ%2FTkzWiaOd6BIc%2BiwISx9r8wuODuuH0BdwnksHg%2BXQ2iK6QpByYNOVT%2FS9chAQIB%2F7Xx0ptsq1dt7&X-Amz-Signature=0477c47f3c9067962f3bb5c527be708f7b1147ce141fda9b89f359e98727e4a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

