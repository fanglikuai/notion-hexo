---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTT66ZS6%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJIMEYCIQC3%2Bzc61VGGey1aLSyP62uU2uJZ1h2iLjLFQLLd21F85wIhAIxGoPMJHc6chQuSyZpby5z5mkrYL5p9mjZfH5jD8oefKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzhcUc7W33TpSfSXJEq3AMpPNj33%2FcrAd0qmTczRK1yKC6Y5C1OJsxnqNrRxDVyYPuI09f%2B4dkxtP0qq3KfVvhJQUgFYIFoZQKB1tTbQ7%2BgOpoOW3hFZk5QYwMJqITrIioTZhGNKwDT6PHDPmWWi6NaGR58PyJ%2BUNAcZj2ArEmp2Y80viRHa1ymTgZIThWCwG8JLHazuOA5I6%2F7%2BBznuGvorfAwA5dOqWCuN6lVA3schfPlP6Am8y%2Bcca2kgmblzcq4CDp%2FZhVvmYLU7dJbPsjKWUfDvHKfWMZDcL44N5jhVw4cpNzfl9Mjw3nBPrmNJZytBhu4EJfs08fMvYCC%2BS84Yq1ILkqVQzPpTlfJLTL%2FTdGRbcPj2P6tVvFj0RpgVVQk1eGvhZ9zOKMgcT2gc6dgfSN6A0C1lW7HgUdXT%2FvGIXl%2Bp48RSKacKMj7j0rtLBUPnsPoYXUVP%2BwsTdhAGBiTOD%2F5OfDT5G40g9yyNLTEUklEfHkF3snbB7WC6mvnJpM93NRm8SAU%2FvA5LrjYk8vvPs0drtLsI63e%2FwCiG2MG%2BQqViRe4jjHWiHLCLC%2B%2B8fzlWKqzkZGKFoLBsK%2FQxUKhLv9p8CeXGtI6UYIl5KmQGW0tTXs4q8pM7JTDRh35DwUW0NvM5edw2efGLDDclNzGBjqkAZyptRPqsC3jiAo4Bd%2BmnrIeU1W%2B2VqiyzvO3kpvCrruO%2F1HOXceiOKrZI7P8sr8EKcsR9jXIpLvS6%2FsN%2BXDiryvM5G%2BnuKqHM%2Bl1cQxBDlqGIwF0m3FtRqBylZ%2BWKLJMLi3J%2Fh6REc0lLNMr6Aq6o4AEBmWI48mPlPDXvtg6HDISTsBNcjzXJRVPSOPMCTnnUtH8pNdIUwPnI8mBIosxXMZM5z2&X-Amz-Signature=41204180bb3eb6431fc8715a9fdf95f065332b593390ee9622efaa7f1a44eff5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

