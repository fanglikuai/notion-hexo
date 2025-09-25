---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WQ56GX2%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDD3xO1IPFgQNirO6B4Kgjg44G6r3IQREebP8418xBA%2FAIgJlyF5qEslEWvb2owFFHN%2BRSfFu4XDrMjKInQ1I3vKBkq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDMLq70AcOlHDRtlZcyrcA9fZVZIEnclmdRh56CppcQrb0Pv5Mao%2FVNlbLCdXSnndkHICbuUTDYx%2FRB9BB3NUNftRxw7juMJ763FIOFI4BV8WeYhmogBcDZOVgSturx6bObAywLd0i%2FDr0UA1rl%2FaAQaF%2F00EFm%2Be5pvyGsAIwZERBVgSsKgiInB2jk8CAny%2Bzhf9SMURC8uk0iXbzVB8NGEOn%2Bv8tzeXp1ZUbbdKLdZPiVBQwJopjFS2jMzWyxSIdK%2F%2Fb0YOqT8nJGx99PsSCqKFe%2FQhGy%2FSRqr4PrJ%2BwFiuUAzje6WD6WPrm7phF0fslynZOK6bmCYMZe4AZcUDxZFNtZrjOmpzjlCsHFbhzbgG1LicTWkLQLy70jH9FIyFf51pWfGXfsZ8G8SQ4p%2BOIMvakGxnvybT2hL6GFywsLU4r3VA0hr9ybvL3PeYypQ5yh7azDVQHKjQOEgWWqJJW%2BngsRM5RfpQ%2FaBBhUqOhRmMmrjGtqi1XFuJQWi1SV7qXuvoiGtkLGz7Z4%2Fqvs9%2ByUAjN1AGFDlHcOEhaeFdDHS9E523BzX%2FMGsUm66nGv3e%2B%2FJjxMKtsbSQUvbqC8Ez8bj%2Fy8qstBMgHD2kD61pQCLO%2BJNnJZS4IsbBbQwupayFblQpevnN1E1pmfx%2FMNHg1MYGOqUBmWaGEgbhbw2toxpVRH%2Fm3RPAp3gR3CEFjjWbOtVmzQsvmKXaCGg%2F8R2Cw0fixh6Go5fBGIWHkEENu44bwHh5SB07s4qp0dCyc5mvsMwhk6DC5e2eq5SbOivTk8zCyZpKiuXv7DoqN%2BJVXjvJxo7ayoOso%2BUCAavQt%2BfjBniTjWrS%2F2F05AapytOvfGd6%2BmlGLBOAsSWaAcxAje7WDCtmTeTTE762&X-Amz-Signature=c0022ac98162638f3ff6390e7ad54c06a9bb2639017a6aca191097097e7ac5f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

