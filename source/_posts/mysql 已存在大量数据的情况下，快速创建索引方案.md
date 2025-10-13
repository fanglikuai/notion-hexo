---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2F3NRXW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDaeJ3FinEv5EwjcVILmbRixVi5JOsHE2R62pS9VwHqHQIhALdzn6hZmiy%2FfU2Ow%2BtntLaIW8c1SD8WFaKgRv7L11aPKv8DCDwQABoMNjM3NDIzMTgzODA1IgwXdcgjWZuprE0KVsUq3AOjEeCR%2BV9DrVe2nEoGw1bq%2FXoib%2F4qKRdBfntgwg3cZ8bas2eZvL4BODW01Tqe3PKKVT%2Fr0vqVjI%2Fi0ieqSU%2BSvtJyGhXhOs2r47JtEj2jwzoKxQ42g2WAtYRVPzu4tlNetiHHiCb99nH6lM5eWg9IZ4sPlncmHa8h%2BD3T4QrasOLI%2FqMLf55k6NmkH%2BkohBYgHGsmEfT1wMIN4t83ZX%2FdPoxN%2F8f09PgT7PqjUBfRtJOBCta8G%2BCPEMaZK6I0LVHfOdcTYN%2F1XE9Hmeu7cRfcIXX%2FSk5vFLeR%2FpDEtyDxveWX9ozf2h4LqyUKwTMy8NKvBqfjgY0xIqtnt4F4D7hbjo%2BBOtytcHr6Qt1gfWGYf1RijP5BnUdIqXgVSfOqz4HR8Z%2BDZgnasx0LyQ52lOBwHVBOIiydtvtViTX8lrfNgTBI%2FeG1UGz13Dt%2FNT1WRzz%2B4JkgeJWXaXsp1ItliCaNlxl7QUf2jS1WJo2iG9JAHIBzY3E9hBznJyy%2BNoVu5eUnJbMCHjXjYsNj5p5JSPGubN%2Fi4p8pGdC4elzrSFU7dFoKLC7mdw2p5G5F4FoM4IpM5cLhHnemml98XlVMO7LXk%2FQtfkU8PKBM6JLOuXcrUNakT9Xw%2F3dk7kR8PjCg1LHHBjqkAZ4SJnBbpyQ%2FgUDAt6F1pTi3qcHFtXonDCMWeEXSmSKPLzBqCvVLjUkWr9Zimo6BfkdrhcjPoQKeUeMxA2wci3J5KqKHDErUVWHsP9GnRBwu8A5MhSsgCbBl10aMkhPdVTl1Xp204wZ2l8xcUHMpcUcpGufICHg0sWIw7%2Fs%2BkwFpv1iOlDwwqERXbKSEPtrm3ngL3WCk3TLDfkyCO7UyzjCPBVFO&X-Amz-Signature=28ee95b443a763103aff71ce8fdf72f2ae6c32a3a6b56761f4027ee9330abd8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

