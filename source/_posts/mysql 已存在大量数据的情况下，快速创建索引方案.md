---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFZ4SO%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIF4ULlqV7IQDL2Lm49nSBtX%2Bqr%2BuLMM%2BTtifucYu%2FmWOAiEAvQ0AK4Al6zZdLD13vKiRTtVfJRAy3Cr37cglOtUxZp0q%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDIR7fp8K6787A5NXCSrcA09GpCNSvSCyjJh3WLqIYy8QqffZelvcTR5q62SMHpQFj0lvuXj4CbrvwBNSXwC7Stvzo3PwkdzANsL34XjaCLiMHQL6YncaJ0ca1w4asHYA5kBugwe3BI7UX67XB3IaoSXd8zZWRnCs4nLuKP%2FKXb5BG23PL4cv4kJmoyCrhIXUsGBnWNqN7MFeM1ivMvyUY79PQu7eR6yZGlLjC%2F%2B2cerO1SQ9xDyJY2q0HyqDjnR2Cx8JpROs6LxQL4P%2FApd5qj3htx5DVk35ZQc9gz9rsN0YKbEkphvoiqk72pEzKSWJvD%2BqN9wIBkjsSY1n0n4pnhZQKC6kngtz%2BeW4C%2BEWt6%2FXOQCqAlAXgT5gyd8tl2VdZqEMPURYngj9rcsnr6sxpXrHopx9kKfe9uin7ZgMYr6b6oLwDkZTtPnB%2Bg%2FJ%2BhyZvFy1LvDFRXaAzcROnWhTcYZ6p4MXrY45wJdvPu0fuKqmDWy556Em5cVZesCmv4dnhkMdP6baCq6n%2BGl322mKDrizDslMp4Wu642ahtReaOoXZrlAufPTicmpUTQhmEPz9SCm99gGLQo9wTaZ%2BfstoQoHJqAFsQEP75SOPU62ZiLNhKZtSNegGJPW%2FPBB7nRdCBNSe2PzfvhPijG1MMS71MgGOqUBOj4vCsjQ%2F8UbTVLHqFsVDWCa2ykjV5ZGaX%2Fz4NdtdsOEOVegxqdygk3LEfx3DdGpylk9ZF97Xyn%2FGSzzNDds0g5iaR4IBmBJ9YW1bgwTmS6ylIugQJ%2ByknbzSiXFuuI5yKHdK8pfbQTh2usZZUUUzL15sO0%2FsR%2BevslUcYoGvjky3UoWuWbLoXncsC5%2B5GcXGcWm6ugy2QNbmd7HBsor6LM12ebl&X-Amz-Signature=57f6ac60a225aea3cae7de71ad64e1161aee98df45398ddd9c643438674772e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

