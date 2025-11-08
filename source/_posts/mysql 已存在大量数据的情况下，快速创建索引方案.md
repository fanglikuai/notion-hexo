---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XB5YAJZM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIDuAVVxXXBlWicaQCkSpkdn3KTrtLd77lC%2FtsX8gEYueAiAuwdYEKneBRnxOSzpfGErWxuTt60C7iMU34Exo9AYZFCqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOyfQrJYftgZCMC8pKtwDfwagpOHDH78MKDhkpN4vgN767d3iTMQ398yQR%2FJGzvnDtPwQedRTr6A5akwfyXIp%2FvRlpTO9G%2Fi36zAXOnDtnbPjw1uSMOOh9UB7xDOJTvq4zCtQs6cpf9%2B4DIANnBCbY6q%2BhZ%2Bf%2B8qd7d879RP4tAaKQfzBoth%2BOYdKwfBkSWLVIrigHWyZs6LCFOPhaGv3wuau1%2BAN8tGJ2bOIyPLO25XFrnDbczwHncJwRccZOTpxhdoaH2Qv4%2BgPtsvZ%2FW8YBvw%2BMLE2sB02egCeqnqebbHShpZItIQ89CJUU0Rho2WGY5LL2eGCWZZQep0xeuVBtm0j54ImwT0i3bnUOtuNaJ2PzazeZqvxemXk5s%2ByUaydQOQ9R5RU1Uo4E%2Bfp1uWUEKiQ1Dul3EaMaG2vnU4rnf6YiKPxTO3AmozSA1S2sxkG0Gvj43e6IKfMQ3kdxzd7Jnvr7fewi2n2ZWLIFHI4E1JSli1uQIm01vsdgdPvv7NyfQY479UPhkR8gCV9P8PI0nbSwX4pAx0o29d8jWsI4kPPY2gNQPO0r4tD4HJX0Ndm0KCbiQVJK3Z0RC8vGMrrAZIgnvQ2BmsHGxIDbxsWHxZXuGTVI7lSlMe9ZRVN800dquPcURwVfvj6weUw3K29yAY6pgECnjRFA2SuAN0%2FrNGAW8175PLgqiqRYYR6CY6I%2BBRIqR3LDU%2F%2FE%2F5Znr2%2FMnQJLjwtb%2BssC2cjkBoWXLpGKR5FcBUSiGWfB1BhzoQE0uWpigmcwDX2kqIqMG6CQdKS5IaeRPiGI1zeWSYf7YYB5oQSLZdz8PRiHlUZlkdq6gOq4cOnM124TzYv%2BlY%2FQco%2FNknrP%2F%2B4pH7n1%2BQ%2Fi3jthJeuLLTb%2FGEP&X-Amz-Signature=5825eec979a8a2f1e72c18fbb892731768f0c804147771f3d08d9d070e2210af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

