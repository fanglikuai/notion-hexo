---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUGPYKH5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDG8WWsSk9duX5dnQ5VNcgDdUA6L2etV8XuUO95xi7oFAIgdM95T4T4u1k%2Bpuh%2FfeomG5LPFby8Bp4RaKWwRReQ%2FoMqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFBxqoBIRHimpcqQPCrcA15svr5T%2FrbnBEMNKfjUYVGD4BIDqBsd%2F2tDu5jkEsuHsBJxS%2FI%2BZ3ULOwbCywTPbLu3w0E9IBMX%2B%2Bq4uQkdbLRgtJ0ZuCBNxKeRygk66Ya9ehxyHhdCgK16%2BiDZb7xcqT%2Bkdg8p9k2NojaPdV3FSHdTjdqGCc1ebr3SC7sk7A6bLsclwiivPAqoFmtwwSpDBmuxxr13MHFkKiaHyx3paM7WYolFs7Htp9qhCDgv19f5x9PmPW5m0rcMJPLWfD8hQUm9WL%2BvNgnU5dl1Adp0mYSM9UoKuCTRnO%2FhAUESZdBfXJNkWF9Ef7dNBv0I9slc0B08oxrUYRjO33n%2FdyXbZDCqTHYKm22qXNKeQG9j1LzQWCObraZ46QA2nTrMs2Zh56GTK7QxaC%2B8zvFDS0dh0krfSREsBi6%2BjdQbzOG9oA4ejqyKtIlpongq0jib5wQqiFyGkdDrQjpOrZHuWFcQ2iwAvBzymmC%2FhULInbDAnXaiEakvuvhC2ukhhWx%2BJsJD%2F%2B0Jx8ZTzrHKqzsOiUlM08Met7ULjSwQyx%2BZrHp1PtNe%2BQ1DpnvF84UqXEp2TJwZUi89oZZ4fWIUm7jBqPdRNLTGhqJat0GENxRVC8f9UKkjNfT1jTSA3dUI%2Ff4%2BMLScicgGOqUBEYProqdYR00LhVup8t8FmIWCkEgkulz8CHZvSYya4C8Kcpqd%2FiqwIxmwjhVMRAcmzeItOBFLj5oeTGKau%2FlNHWlyujiZ0u47A2WD7N6UtJPSW%2BDp3nMwBgORVSkYv%2FxI%2FdwKa0GApGy%2BafgwUwdC4zCEfh%2BftEIgydi%2FkLVJkjUoU7LlBzwV1vzgjIf83iRUEKguhiazrTkH%2BWC%2FNeM%2F4ZKrPRDk&X-Amz-Signature=402785ad4ce7bea66aa1879e11700acc52814edc623b597c96b1301e96242e5a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

