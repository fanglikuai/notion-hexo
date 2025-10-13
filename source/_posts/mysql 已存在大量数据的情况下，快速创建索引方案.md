---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSAJYO3Y%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDndZ4gttarasSnwswoQUVuh48eyykLynN45GYRTgSEsAIhAKWw1sXPFh2ErtHKQvrsDCoU2O1%2FGVRaPcSQDF4R5eN1Kv8DCEsQABoMNjM3NDIzMTgzODA1IgzAN%2FToGuWQSK2%2FZ20q3AN5YHCO%2BQY%2BouMdWmRDnoTBZa6RSVPAZJd5BTdD%2FqB%2B5DT%2FAlr0HcYGD%2Bmn2GevHKSZ09jVHf6Dz1RArXBZHugOZxmEBYv5PIjOxvK1zjKJfYBkqifmJH2k7sDj0IGLD0q9YBSK5tMA9yH67Wbfi4riF6PLn5HFXQqcKN4ARqga2FdFdNv3daSfZaY%2B019V%2FmMpr%2FzoxZofP62H5pP%2BQOi%2Ft4oiR%2B7u2GTyuoKdgXj8UL9%2FfxyupDJbOXxcKa4bREjeN%2BK7SkUn2AL%2FKAYjC9kVk5xAr3R97YmxtU04w1TTeZ9bEoTMP01C7ke%2Fri7r4LHek%2FtXzt01WOXIKRJvFpsd8mL%2Bqr5Nnfdh0rN1RxKUAcNbs2mmbLu4b%2FVFxNDAlyWJEPevdnJXKZCJYc5GVvKB8n4E831k0oaKCyAmJCEB2Lg9f0BZ3VwBakBWEK4t6npWudKF19j%2FVyAjKUp5wBm6UgD2L1mX7GknQuX0kdlhvf5d8OGx6F9kIPXwqHQzS5Mc4%2BWSk9S1RK%2FrpW95nPguxUYj4JBoG5KNblE%2FhYldK78f2nsFqDuUYRtQQbxBCrs7keLuECacqpYnuUDtZ09D7b%2F6PtWyIHPicpA%2BTX26d9Apk7pbo3aroCgCzjD58bTHBjqkATswyvbf6GZS%2FlzPhLRyRwxs7gHvA6fF%2FwSLHGgIGZOG0BXSugOXUxYAOJvGKQ5UN918BgRavNgyKCPatE8HDu5O91JZO3q4yOA%2FVG5bldyQ6zGtCGuSX%2F63Vrew6R2ErxuNUvovgRwc%2BN0edtp8tOp4VObl%2Bo%2BTb1hf9zd6wMzVhihVnX4MiMTk8cBLNIp3AJTtjJQO%2BhwXEkWs7mbt%2FPN33YZG&X-Amz-Signature=bfcc205b0be5252a148caedd9a2684eecea9599916f29365c79b362cae9b14dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

