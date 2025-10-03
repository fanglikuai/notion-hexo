---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XRQNOFR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICj3tszmmkkQtgyJb1puE76S0DMPZlH6WFX78Qq7ZIG8AiASbIlLdERVZChYvPCcbMryajCGPJEUJwR9e3U9GTvX%2Fir%2FAwhGEAAaDDYzNzQyMzE4MzgwNSIMYlYvuD3C7mVtP%2BrhKtwDdJV%2BqC%2FD9hTxVoiXuckj5e7lyUf4d2oEoFq2%2FbnKUkwUtZsbfvd7Y8T76c4ddzQXALYQ18nMBzaZ5wDUVnczBHBLaNIecu47YjcM3K7yGRSr%2FzLj6MSxL6fvQdifdWDwKSUReEqR%2FIzqdXtFII69mRoWj4goAZ7RutndoUa1oESpLqqSDcEyI%2BPsh8H97pA9AeXwtX3Nb89OoLlecPA%2F%2FOknDRHMHZsjdoJL1%2Ft4JtP7S1yjVWpPb1jKAHZgbY3XIAs3fxcjUTKdBz8GJz6e9Dj7mIifXwmg%2BkcZGSk7mzZWoP22wrpHuSyXcdV4Zlnz0PwNtO8NjGGU20fAMVdFLycb4LJaAN1tmhpX7znv7BXJ0V9CqvC8%2FsN9n%2F7EJHHLfHMNMqYsuIzOn%2FBlwpuyhmkbAdG0j%2FmwrnHLTrzbd8webqEsjnaKRc%2FbIK3nBx9oUqcLpKnTR17%2BliZhYGJk3YIzwCG4p8zo3q8XtB8VywiRoji84%2FxNpy5rKXNAwB8B2ggnIcqJdioY24lf05x4JaATIZRYWLQVE85e07JcyMSxFOQduzb7Q14Ai5bv72e902zE0UdtyqxeXxzB3GSNdqnLPJOcxLMprcpc9Nc9zX3LLy%2Fz%2BaslZNcKKAQwmZL%2FxgY6pgHO2P%2ByrZeBhqfK6vYH1MaVNhK5Eeo0PTDUsuBdHGGeYdPqQrIA42QMk5KqY9SLw%2F6Ki5xQYXCb64r9LUyhzFCIoXOrC8UmwEU2M5UeJHAy%2BnKNSqCEYXuYpfMiyLE3QIZ6zB5DahKSkylML8JIuQKEIeoTRabifI8%2FrZSWgs3FjpcxgCFeVjkmr%2F8P4xuqATDMtwcftOBTzGErsEpisU5ZoY7SU8eT&X-Amz-Signature=16cdef748af21557c8330f98f7fdbcfe712aa9e2619a0024fb4b262b2753baf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

