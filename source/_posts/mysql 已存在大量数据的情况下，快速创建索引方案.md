---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LQGKGOF%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCkN%2FtN3quJVEde2ZSCl7F0UtRvwdpWxlxx5Ga1lGMoJQIhAP%2F56DJ3iB8j74vVHRuLsqJWa5za9WkCRy9J7YIk3fN1Kv8DCDMQABoMNjM3NDIzMTgzODA1IgyTV6lspbPNOgLzhtoq3ANNbdYMyCWP56g2K8nGwxT4AjrNbOEqgMgBZRRfbYdi%2FOIMdqmlVbcGWnzPMDu3l8TZ9wMuX4GYD7DhLovZ%2F%2F2Xe9mdb6GFd7S5EcdxFv7KmuxYOvtxo9cfE%2FDQhdVrqY2jhrU1i%2BduVLYjFxM6h6gwIdUdQJLKEc%2Bgy894G0GsN1ZIZX0VwctSkJeZbtKbuANbkkgxRcEsLZOsMeJjvRS0XE3BIaxq%2FNQiA%2F7i%2BnsP0hhwB20O8C8%2FPY7E4k7BUdXBnonVwK2MI3Q25g0nnZerv2cBGcS1ZFJIfNetCx9HbjzbIq09U8%2FBKbUJQ016YBAXPRPzuq%2BlgQBmDXHZvev3qunOSMhKCTBsEoyYOczm4dC9hJwVgLh%2Fn7t9hp45uQqJY4xyZIQIXSfnMzUquNFI9rSg2%2BE8biHyvP%2BqPraBOgiTqetJFAcbGE4shyynEVSfX5SxLHmCOaQdEWAohvNdODSWYEIa4d4PPyJ3BkEQaKwwYOwHN0eRjYyLe3aNkY5ey5nnI3aCRFQaiJSn9Gl21MiWot6R8IDc8dhJ7z1s6kMmQiFJvcnSqUmAQGwRihEbh308hbEit0Q4zrJdZXT9Ix%2F6fr06bFGX7IcLVUrjAT33Atael0oGhR2QijDHt%2BTHBjqkAb3zqAOlVq1fItHGRSGM1f6xDnX5JrAO0biapnDZqqVTB%2BI1AJFAoo0LyEIJ1bj68BhDbd%2BW%2FVYxZdBxO2AVJmZvHyK2ksnYn8Q1l7bOs4kzjU1oWwbqkW5E7rI8Nne%2FJbZbNDiYuXApknWHCi6aWeE7ybtUBoUAPv2UuKc%2FOpQcUhcB8eAOnMRmZVXEvNnZ6Kboq5v4hXe2skAzg73XuB4wYqmG&X-Amz-Signature=13e7cac15a8765414d71e8fa9e67aa7ac06b23893fa092b2aae7f50d45d7becd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

