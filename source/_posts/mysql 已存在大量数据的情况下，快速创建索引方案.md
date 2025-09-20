---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MDCQH6J%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCICmPBlh33mEHWE8B82%2FttqIcrHjFajUFUWWl7xTYRghwAiEAnAZwSoi0vbhRgaL4%2BNrGakY%2B0HKUv701Y3huhyNLLKwqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8uRSjcywj6yDPGvircA3ZMHfxVky5CU1QuQGx5Wk6H4jRwSLsPE7FmjOEXjuqzUvKQ0NRtNjXylqwiHRpS2Xfl8eAQZjAVhDserpMMsXdzECss7mXNNhb0qkhxo7hEajYGbL3RpScDjf%2FI7o8PLst5CWdyKwllZ8sJhU1kiqMwVK%2Bq4PPL%2Bk7r%2F3d8YWkhEi4TTq8ezZ6CFi1zKZvH4XAhho2rGmm9e53pNijadQiSfQ%2BdStCqT8ODaQU8FClnfPneZSC89ALSWvnsnQ%2Be2TQKfqoMl0NnOxMP74SFoA0lffkkSnZ4DYa3oWX2JUx0%2FDBvRBbTwq1fAg78iLH5BaWmiUPuCR96n%2Ft0eFBZE3Np3nL1X6EWLdhx0t5W%2B2cF8KufGdTP4oBJ%2BX4vkwu7kGBx8Vam1GJq3%2BlkukClczt7MveGE4bEO6QsHH5symLtwC5y6StbV09imp2zCymkG6BZvlpFVB%2BiIJPcXaTs6EV49Kvqg%2BMjSDfhWs03%2FwR3RBuK%2FUZXRHksGGDhP%2B3A9L6g1tYwCxMUgGqKIPx6BPR8mAjFKVsTtCRx0n9UVZyr83VCXbiJ%2BbQIiFcQImnD%2FSTQZ4vQjl5FGQzD29s0FUL%2BLJEBb79%2BwHwMHIbWmaGAVUOm6nBvqCf2vQHoMJilucYGOqUBPBEkFuhxdzK%2FONDXY%2BdRZMOtoa46ES839XOW6dtR%2Ftld6yTbQIbmZWrSpLpxxRvO2TrUKdKnq6PmSfIkOgvYvPIY55RSJUdARAIjDOWSYiGK7dLf12AtpsBxgS6LVdyReSLSiNxikI5e%2BJnwm8Xa7GpmCT1Oo19SQBxcQNzJ72ckHW%2FOZp4IzmZ6YiHFv8SJXXeiotoW0NqTXVtkr4LXg09DyFYq&X-Amz-Signature=2434aafec9edcdcb2f06c933a40fcd5f66186fb21a12371858d69660e678de1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

