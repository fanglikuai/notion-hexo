---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFIOGEYO%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCPDhD5PraYDJj8hZh4QQpnsyzxW6vOjSX%2FjTTYAkpiIgIhAK%2FK0iQfPu42s6%2FcnHrraSk%2F44J6z6bJXJfmCvtejZA4KogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyCH%2F%2B8vWQCWMaTeKsq3AMQR3gldFWdK9WmdVN7VD6x939wdFiWtkWgSCmVcSkweBtVQnycAG09LclvV3A2XerKCxgxRVSaOuExOgh2Dx569k6L10XyYEgMhnfkwMq5V73PV4kR09sdU5LnUjn%2BYe%2BGTZw615fxrZrT%2FxwOxrgXzbCnmAK%2BI2MDE%2FNlah0nQ4GQ0qNNoWqvf2qisDDUPMZA5pg61VhQOC18eOxJDyoIXE6nSruHIHYrBWzX1w9P9sYBFqMkD9fWc%2BgdZb6HZ7bt9O6v0KvOqrtyQ5pfHUVqg1ixsPrNKgtVE%2FwAodzZdaEXTr%2BkJayywF9zQipy15s%2FwdCv6Xz1sZr1TZh0IZmIK5GxRDch2dEuqJiQN2CpRRdnEckx6V0SSNGSOWYiAJMq%2FqeRcq7BEGk9Yr%2FSSZsJgaYonGGSozBicJ1Roi6Dq4aT%2FHqLta9Y1o9VlOALhXDhNQLUYPy0OvJETdajNKQULygFZSAgHMlFA4RmBiaW58n4CPq2GUvM%2BzMObfg9LA6Zmi3%2FnjQhz6Z2wsCBecz7MSX%2FbRSxuOUzas5bDdrCSh740FX%2BQA%2B9iPl14b6KR7%2B5xCtQWD8%2F2DojmbqWkGbDzZdS6fdwi9Mi8jNsoeP%2BQxaa%2FSvNAPHBUfSrDjDRmuLGBjqkAQ0Lid2xyU9IP%2FxOzRTS3qsBx43mJo7PNk2ocuj7gBCyP%2BWIigvOTfQPDm7aKgDpIE%2FJ2XEUXLIWtRMKl61OtJKJXHz3MrrlBQKqP8WlzL0yibDe143gtbshrF9EOthyE11LVAAM178QbPztiHupOL8isKbrMOXnuteflPpgdUtmtKxCbvf3rVxk%2FTUk6sgelYMRLuzGBNYybUuEpnACjtAPi1P%2F&X-Amz-Signature=a1535cb7803a6b79694b0b85225ceff8e947118344da5267aa9b5ace362df747&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

