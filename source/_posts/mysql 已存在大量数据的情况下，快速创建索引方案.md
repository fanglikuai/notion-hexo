---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FLIQAIK%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQDY0krBwkeajlAMDvWeCHfk7o6%2BTLZzk8Ezft1hiZuV9QIgYxqTrSHSALIbmxxcykdn5D8yrckc3adzw2MaBps423YqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKepHBvCHmp4k88u1SrcA1MQDLhTjdaEPpb6yH%2Bj0r%2FL8f7nRyL%2B0mWG6hvV8TtUJ9ASgLFsKTsxZ37rGPgqVzqrpUBbTGGyKQNmipuAU5EahfKha%2FQfH8kFiITuGtLWD23yo40WSB6ojjrMfX9dDOGPmBZodTztt4U0a0ltqmHK3BLJJrIQulzOEJKjiKbP%2BRAfLstI%2F%2F1AEuR636%2B7EYKfkEtLqo2NHDi%2BCnr6q0P9ZMYkr%2F1MTdX%2B0d5qqazCR7YAtEP%2Fe8IPJDqEFHnTesAvO8rghwe5hW998tkEaqwtMmiE4tr3N2XHmaFXjzZFKVlfdI916Ir3IFmZlFfylA5ZLhdnBmAL9nC%2FtZ2nWHtYrdMyEOfMDBUPACgejuhOQLGXdnJ61HWor4br%2Bh%2FQcV%2BBVjakgbaAjo8LJOxxXAmFH0nFSfuyaxj2DRCqALh8%2B7Exm%2F3TAC6SolojtuWTrBp5aHoidedVXU9cRfQdSF78OYe0Rk0fs8Zk3wLb606B8EfGAdWFArZcEm%2FOcBlXCIRdEzOjHUwZ1VcidvCOn3xI5J7xmqu%2F5dUnHL3%2FfgMhcjfe3eGPCdNYZO93si8BAQ9ZIPzTTz0A90pUaRdq%2FMnTJQlGR%2FMcqwbeMm5HTjlpCl68dRFd3wXYSVrOMOCTwcgGOqUB70Z8HG29IjnhKHv26q2CQyo7Q%2Bl5amfFq1RHww%2Foov%2FZe3eXvhRwL1YGhABy8%2FO6dKt74tTfTiAHc3H94ACWi1mdeYWTguIkawHylCD%2Bz91mKcuaikSdL1A4Jh39pGwIOETZCdguV3SzO2RLBecjmqJxQO0tlT92HRO0eueyXZGcStsAMBjKG601tkF5J%2BA7Lv5EIV6IS1TZ42yNXh7FIJrkYgGR&X-Amz-Signature=9156e578b02ec9e2cbe364a374944d17536cec30d199c9edd2007ac5cd8e2d4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

