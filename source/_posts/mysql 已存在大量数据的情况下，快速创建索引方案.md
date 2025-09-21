---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3FMGM7P%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD73ZI3J1Y2%2FknmsYKNexjSgUOpX%2BprI8uH2Kg%2FvzPgkgIgdf3ykqU8B1BnufPooT6JIDJ%2BC4MRqPwbisM8d0mfVKsqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEvwY3a2%2B4S0YYWFSrcA3wTqsDcednZoesHeqr%2FZcUw0jJ%2Fxz7%2FHgaikkbIsNta98k1TzyxVWntoby90webOObTJ3fmT2YyxywKsP4CA2%2Bsd40hWHFvWnF%2FZskHohaXOL3zzNMqvHqJk8zURYp4FTU%2FHh9XAAzdjmlcj7cZp0jBdWPZO%2FHYWpyVxRWA8LVowkqrHq56klLoVAIye0VICN23%2FZuFFmJpONs7LkVT6rdLjDkkwgiHGUJSlQ2awX1I48RTapXzHW5dT23W%2BLo2uvBmD5F6S7rC4YnpWA6Z59mv5YoUSXG8igsYoFFScobrd8n6FNzpAOcQby4G0xFb%2F6qurcj3g4a7iXOuhoIyFgSkwLXIQ7VDbh0tMJcGspTHRviHdL5rBNjXlDqjpcT%2B4GGY5iBZqCzdGN5tMH8xfwdTZlPss6EzG6CwKgyAsn0L%2BE9%2Bc8IZw0YzrqBAdfhbwe8rKKyMybIoaWG8hWDVXgngXRs1rW%2FgTFDhlD%2BFW1%2BOh5ggEj%2Fm0ihSVVYi8nCvcN2zRN7I4FiJzDiCtw8UH734LDL0Je8DO4mhvTH2ykCc%2B1JZfp35gqzlxGgn8fXKHNtyXFTgQjNtswsWHUGbKrOxFeLQUDNDsRwsD0X4aV9Ln%2Bzz1AoN7AO57W8qMPK8vcYGOqUBDT%2Bbwfy5m977AViP9DFJ3APCR8tMHmXn%2BQQIxPdYf5q24W9hko44uob0FYXrlWcFtYuJJx%2B7hOgCjIrn0cYU6RXD%2F8oWV42mjCEo1VEaegxiNdiNuIYtOXPdodX9BUVeSt33Y6gV3kOa1lByPJh7jf8M0oODnjcQGX%2B1S0ClzT%2BHlpRfi34p9NlXET0SMd1Z8xioSOd8jMcxtTHW%2FBkGv%2FBtkhsR&X-Amz-Signature=486e9162600670d40cab73e9d06e7429ab4a88bf8ddccf1bba15c199c90ac382&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

