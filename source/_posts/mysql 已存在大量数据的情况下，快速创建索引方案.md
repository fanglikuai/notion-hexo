---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSYXVICJ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDopm1Zh6gisFfXB7xztnXv%2BFnInBbAjVWdMOOVMiEDFQIhAOto0oYp56MkJfuiZTHUX8R8%2Fer%2FoZLNzSwSYi1ThixjKv8DCFQQABoMNjM3NDIzMTgzODA1IgwwV81IG%2FklNoMfqPEq3AObwWQ%2B1Zoom%2FODRIPDO4f0eRPwBAbf4FTnLZPeBjCVbjheLz728tEsWL9NBTqhqpLrS%2BuJ3AQPZOTfy4Wl0K0VJzm7RoP%2F5LEhabanP4yyjdIpKcIRK2LWxqaUmmx7S6aTGsHp5od%2FbAMMI9lIw4S2GzKAVHpJb5OpEHrNfZczeIkQqz6PzEHAgav0js0ZyMkRBSxeb%2BhXxIg70PpJ4VI4X9JVu3tBEE2hSplbFrsZl4maV6ilMHg9hrVjSIAQS6lA69dqy67U6yuTXRbJJPYq8OCxZApOKNfqxjMEm4MLdxGKv06Yk2ntpZPBIT4hNJj%2Bau9qzrjJC9v8MpetMLYBbJxYmU1GfrcH5FTFg0t59Amjm8aUEzuwq5F90ZUlCBFEx56Im8qYDx%2BrXXoJp5Rx8AIIEoyzIbw4olYXU%2BXaQJJOQnmrvSnfJ7aAEFZ0HszKL8rr9BN0%2FrlN4JkeVrCQ0O2fidVwqpawZkCSQ2PhnqIzeMmDmlCGqEpMHmIGUK8u%2BgmLckcsopXinYwEaZmQl4FgM4SY8KkGPHFwJ4tBBz5B7ROHUeDiXSIKRk7PPhZTNASAEXZMJu%2BAaWK%2FKPEW7z%2BOcFJli%2BEIB9xJkHwfJOLqPcwe%2ByJMpabx%2FDDb%2BbbHBjqkAZVf9qy1kNRLg2reuJiy3ywTNYtDC8VEjy8nj45fVyWIkTJwJI%2BXxqKWZiesH0woI1orsgNWU%2Fd4JpcMUcMzfFL26dTmvff0983ARPPIaiqUgIQppaeXD%2FgD8pQElClsoxoutV2YfF0ZxUN7cZ6ok6yG3r6KlhLlsaWPbJn%2FM4REdgwIgV5akvmBODFow6iejIdakFUywTNMl2tc%2FvbvwZvhTDp4&X-Amz-Signature=6cbd39abae2638bba6b5c0976a7a3df31f6c364070aecfecc306744b6f0ecfa5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

