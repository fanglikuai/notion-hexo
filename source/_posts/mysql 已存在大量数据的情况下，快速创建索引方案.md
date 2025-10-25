---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3D2N4EQ%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD9GZ3a5WVA2vwfCn6BdJWVlIppIynTgD7%2FaDZ9%2FZV5swIgFQ9VBhQU3YrjzUZHFkAa8462xieYbKOcErmb8a6f03sq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDBKIlD6VTU4ZDPYh2SrcA65WysDtEZzZMl8mNlUBL4lrhHxbLkhEetJUcM1ij70%2FfeqxoHDS2nrXVpGwGBtUOG9m60nWxfZsdP2R9LNHg%2BT3pYY3ZDNKqILvsCfWyi0u3zvW0QFtRmebK%2BVG%2BYH8EUz6A5xyKHJ20tr7Mex1lnVlQW0JBWbWtQDLzcstg5MuSdm81VZNf%2BtSxAYJ2AjUuSwASCtF4gq%2BFTetCtgZjIl7zoHRDnuXVToY%2B51GzlHAz5niXIsGKxDPsw0AzygohrF%2Fk9do7seStczEsb1W%2FZVPLYVTDyS1UQW5ffM9Vyow5szsOneH8yS3U0QP4YQMn9iFWQiLuXk85SNlDuPCupqiIfVSx%2Bdn1DbyNc7jLcNIT2ju%2FVr7%2Bwpe%2Bp58KAN0Byc0zjuDMvTTHPGGjQx0Aj6YqHvwkrc1IVmkr1qQ6z9Yqh%2F3K2nA6BBxklZ%2FdEOTgYC6zbFimBcc0qrlzN0Pok%2B8Kg9a7Y9jBb0fRZ5ukhLIf7tJT2Oju83hUTPwpnLuJ5SZkFr7QaH%2Fv1c49Pt801qj4oRUfOnfKzLodbVX7PO0ac%2Bzc7i1gqKETtJHpUD6%2BcbotHqKcbaHpx%2B9T%2FepzSLC7d0o3eizrX3FURNuIOqOrkdEIlLW9pQX4jvEMP%2F088cGOqUBKPV28kSpSvmV3SyOtZhDEnf9zpbYTvRTENkoLCjUsx5wNewxZpYKXqyk%2FE6hYc%2Bogw1VB97Ze8MfzS%2FLPu069qrRZEB2ePzj5KKKyHlaNCABOMfA5hz54EvIEEoUnD2UJgCVAQIyCa3vIt%2BkjfEWou8AJ2B%2FglTw%2F1zWpJcPHpoIZihGHa5vhRjQDPH%2FSJIA4tHlD%2FQB0gacnW62NocbGg3V1Eqa&X-Amz-Signature=5f451f6ef55397187e3e930acd5e9380f475601f321684ab6b2e80b8d81f8e57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

