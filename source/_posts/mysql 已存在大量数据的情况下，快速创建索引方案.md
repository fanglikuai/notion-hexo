---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XJ2BW4C%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQDdYcgo3NmNGkDqG3mPol2hI8Uoc0Kb2sOVhVery09fkQIge1iOuRx7MLxHTzjhb7ricaVIBf4ybmdX%2BNEy8Cn%2BYP4qiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBJdormG3Zf8d80rCrcA7I4TJvQ%2ByQEqqcMXo35TXn6%2B%2B0WB0C6MbmROez5nhfoj9zZvBe5DrVrmEC%2FmULc2GJUxT0gdIwGFKSZa90WfL0cheTAzoI2Z31f8Q8R99hpytLmaq4psP6PxTV%2FuOqD%2FSX5sV7y%2F1%2F%2BvlSuBIAzbagEAJrvTaS%2BKuewZQfzJji%2BQMcSOdwsgv59JuGUKw2IKOPSwSENTfEjlYIjDL8lyCjpVpuSfWHeYlkqgFQOJhq3yQZSsYowWBkG%2FgF9UkZe8PGVvBVUZON2oN4prf0pdoDblZDzM%2FQdAeR6aCUqb4FL6tvUoeVu9M4YJjgbtBAOax0iKrAsQSvMHDE1FUvufcxMbmTKH%2BpONip01JEN%2B5W%2BwPxX74FWQhEuytHPf61eVnEaX4R3aOZDSYqKUFuL7o5Q2m0YWrCLNGVEWCLTNMO0caMhh1ul8YBHPAZ5o0z5rV%2BqCrZV83VfBM3BbCFHQkSPLTl8P%2FYnYt3o2wDERbvM8doZHgE6VqZ0pGigy4S0UO%2F1vIBpZwcnKCfwiDCYffSos1%2FUDvK1ajVEXYt6VOW6DFwndwoYh9efeJ7PAPJkg6NQR%2FjIFH44jr9gg4ZZKt3a3f%2FW5uKVl62ix70RWR6JAi6FQlrx5WBnQuknMPPy9cgGOqUB0jMhm6S3ZAVAR8U9bAYS%2FcIR9DsLdJ%2BZNMkCSwSRLz4%2Fh%2BnWAP9IHlSNgb98Cw1I5PWwG2ikAg%2BTobJsFoHY%2FB%2BJajx%2BNfTTUQoxZVKxsQ1I9qFyhalw1JS%2B16OeSRQ7lKiYrZzgU4%2FDKfDSSq84g%2BI1R2h5Wy2BRw2mRccuUXIkzA7doquWw5unWDKzhpZG58geX5ZVKBsKTJLITJKOaej5qlOB&X-Amz-Signature=6992fed0b57efb19573d628dc49a3829f440b36fb09f5e922f9f4d7b7cdc2e37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

