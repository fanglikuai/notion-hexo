---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAA75627%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAV%2BZBycphITmq4HHF4izO0TeDY1YsRR5fyvQnJalYcAiA0ri%2FEoumm1YDGNP5WzbCGkuSSCSBgiTHrevMHRSW38Sr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMdsA0nXR4l3JvOVssKtwD766QjXh5zLmKmkxB%2FfRTq6xynmNlf9WC5nR7UrN7Ih2Vh4QGBVwmeot%2BAdC9coQMQfkEFdkCYTkxvG8AeaoeJShJTSaEOnZjHMklgACnDts2sG%2BtoOEX0OEnqNa%2BLBu9KFOvCSRiydUEEuKuKxxEUe%2FqmiYLJSk1Zhzw2srmE%2FcjoUb0iy6XtXGBe%2FYoCEft3rrvqtPVwh%2BrBNzeDK%2BPUfRb3ZFF5KCaX05Tyo004mWfPHoYqSMtpXLEF7XSc85900XRemqHV9howqtVrP4jDIIlCuuCi3g%2Bbo8hp9Aznp54ZVhUofd2NsQv8wAdIfuHJp2RRyGrpmE1Ljd7deptbFCBxuqTGi2KsLvC7CtjcwUE3bABfetPZAr1b%2FHKRNmFymFeBySQj%2Bdm7aRKOIvFKdgzbsY9SVOpph6Bv66gyYNockUk4w3QTznuXMdPkq5Ybme5p20xjk4%2B2c8xW1vqhi6%2BkS8R2MxZbog4%2FVgxo0puxCpHFmn9TwLXk2aIBrqvsbTcJmgANkwGumaE5172BBvttlfytn0HBmBT%2F7ptEzSY3ovX4kF8pa%2FGJzrEfrv9a%2BSQbQkDWgyRLxUNO4un3bmAFsiWsTnHrPBu1HgRshGoFqcOfcBIgYlMxv0w2P3QxgY6pgG1NHCgwqoVYhwvh2A2BVwO%2BlGFrX5GjXbrrNcZ3G0GXmLUjWUaRiX6Ce74WrAMNeQ2MsqKbOgdTwI0%2FeB0kDnou1QLyXZfCz5W8iLswa%2Fv3yzIdLM3M1Kt0qKowQQVhUm%2BAlsQ7v%2FL8msZ85M%2F%2Ff9ptGu%2F806z1PkfdA%2BmyyWhAj5XuByWnQ7pcw%2BoXnNeW2KSC%2FgjuulR2PEZfVltg8lauvQ8YW5o&X-Amz-Signature=be0ca91384f01c1b2b46997549e252b6d6c97c7e8bb112429b340ef4b274f7a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

