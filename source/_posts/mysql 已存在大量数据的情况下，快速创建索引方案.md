---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMKIUBKA%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDichzHq%2Bc1C7aSVYIGqWa5q8CGYacXCnooYpdvt8C5XwIgeKvd0oNBaigBQJMUcHCJcJlWfHdF%2FBf7v2rAGRF2ew0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP065v6zw4hnXu57WircAx4ckn7tu4HUPdVjZ1np%2BEmVVWNnTKer0pHxz5GMx6xOZczxxB8IAOL%2B9zcEKgDcUZaf%2BeCm5snAZFuGxf4KjDRYaXK3Rxi8b8M9BQVfvSQhyfPCyS6Z9dg%2Bt6uxMuBRdfwrABq9nVBLeOL%2FOUVIFcVV%2FMPjVSWp0JRUptY4lnSGUrvY%2BFrFbxWFhZDZtSnrQtsdJGWqM50ymlUdGOKBaeHUeRZkY6bpqxnDyjipTzUc5b%2B4jHuoQEYjw94LKGoYwZqQSNQdWSuYU1Ci6QlcCEyUyzQrwCFWmM3D4On5%2FwY5tbgu5iIcg4HxiOyWTtOsMphBzZv9DSfIf%2Fv%2F23PC4wUOG%2Fd6WPj7aM%2BaVc%2BgPyqlDpalNyldgJ0MJJQce5zxxNFIReIVARk2inNgoPiJcEKqo2kdP9%2FYtI5hHaJF6rXyyxR9qM4xb9OMRG8kD52COZ%2B72Tod1DKdhZBhYs%2F69ohoV8VXtbLOnERsv3YpIUAl%2Fgbx%2FUakXXMLVJvEpJbB8Nh1BJMa71L0%2BkB48Vx5XOsAedtlJLNQ19s%2FJL2C6wG0dTBWlSqBfYG1kr7tMN5CxZZdqFggS9E796%2FcVVHU1Bb0euptkbp1GhVh8NpYyQ9VUOj3Zh7W6PIjjq1OMKXD18YGOqUBDj8av5P4QQHSWBCugHr%2BGDmhW5LsIbDH%2Bp3ouLWn93t%2FbymuqLJr2S3e5WimwuCOq4xNjh9jJ8Wx8Vkxj3UegJR1rlU7ExtDKsQH4NyxDdw9zpJMMGZ9w0Use4caE2aFlKMrQRN%2Fv6CcaOhYcX2ESjevbi0BuagGqQPLzFPz%2FenforcHOlr4g%2F1hYeAuqDKgGhRU2VsA2ng5iuP2Fm5aOvh4cMBL&X-Amz-Signature=af4ebd6ef41518c1be5ce4f0580280a6b77cf679e84846e9554238f2bd99755f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

