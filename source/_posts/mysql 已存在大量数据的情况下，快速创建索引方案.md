---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PYMLA6P%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC67Vp04kEW9WeXSMC0n1QMms85%2FI9dbEVu2Tkt7bygfAIgUiJRXtD0t9SZmvDZB5799lMd3oLDDbGnsaoogqICOg8q%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDOFKNaC7quVx%2BV3d%2FSrcA8gOzHUBuEJXr3OV79NyB44c4C%2FEAZ7MdqHDC%2BPSenYpYxwPk2pjrrQnsnVPrARVhc%2Fu7totzvhzRLM3WOGAsgy82M3jo9CdADFOiyfa6rufsLNZTmUdJ07QLrY4p3sw9X41UzXiwYwLs8CQFc9iAn%2F62qUgzuhJbBwpqHX9jORIrrcqzYEPaXaSCbSl2CDCUFwFLXQU7sdjE5zJEE60pN%2BlGbgK7Zd5n2HUVJvXXZ9lB5NaynQagCtqlU49%2F2jWNvS%2B1%2BmUB7b98bHuSKu4Gp1QwXuSqx4DB2n7%2B5SsSRw5HDZRVzcIXCwQ71SnBVcBpL3pYqUt6D3%2B1Vd6BcGh6l6WdNKFhfN8v5BqImVkzVYnfgpp4ueAJ8Vjj3LyrGf70JayHoaxT7iF%2FovUfvS8gebiiwNQaWYVyojTWcWyzBZwwcY5xvJJfifjeHNp6lA0pJDemy7c%2BgbYL8fVhPrm6LUsLnIx%2Bvg4R7%2BIxyrlfHxLwyzmsk8w0m9kE4yCDxF0xOnHp5P3zHIwkkkws8txQW%2FIDVsI7itmUfKmqCZYQNoH8TMM%2FIzL%2BZa%2Bkq3MvOcf2OkOgvkqlh93BusaPvO4BV5G5O1KRNM%2FmM3b2shIlZmC%2FqWm2ZKgDFc7e8hFMI392sgGOqUBNODUjCnyJvE8iBIt9NdhpHoVh7GbzqY85g%2FHhe8x2rUeMpj9Uf7Dl3GpVaxNHt8ck%2BqCRFm6rC3uEXTLrxLRyH8mNCZkN0BC5Tssntyc6UpByWgP0cpHDwiDIQ%2FcoMeQeljbb2MkNv42lB9tML0Uk6v5ovmsyd8vM1LtoVDeX7zqcMCs9uInVbzWnSk5fdws3g6kPOYbyaJCfOEJRqr1gYzXKT9V&X-Amz-Signature=e5e8b4c1a74eba9c83678d0d734c17a96c4823b0ffbf3945d9e935290a0fac1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

