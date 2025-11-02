---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SIIDKU7Q%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIHC0gg%2B3R%2BXLd3VCskeS7DJRZ%2FXNt1z8fnRBVvpm52zuAiEA8sys6zj8B0YSI3qlj6RBVDp%2FfI6qiaqFLPB5FktOYZUq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDCzdGcZPqoVaXylWuSrcAy89AQ4%2FJWd8xc6cRp8UyrpsxBVmftKFzXzjNlbbxEhNRcXQxsaKVzByz6ofT8sVhbIUkVbY%2BXtVBbMHJ94KMhnlw4V1WiBrSv%2FUHJgZ4RW0ckE1FCzXCaADndxQItavTNftKWa20M7xFH0qFygSzbfa9HL3q9VIgPDgLvBuw9WhoiflsCVYwXGJFC6PY%2Bb04WK7lwReWDhJex5sTSrZFePm4%2BWJtpLJYtA0AjrHaRVgUoaIZGcLg2GxTVB%2Fy1CwRyGf7cozLf4MsKEuADY4ko57B6G6pIhiT55TZcOzcPgBmuI1n34GvfZPA5CZ76PgyW9ohQGgm4Zn9A8rvgeyY3Ml4qNi1f%2B2Hb7TUllWGKDTX4lX9x4y4CFZyW7VW8H3JZ3CLX%2B2s4%2BEZE5Dm2y7Qv4E5%2BSRTJbNZgtzbAl1G1fGuLcpVlEjDGZvisZO9xXOd53q4lEgX%2Bp2XTxzvk8yy5lPHIQ%2FDMcx17ZZ5jsyVNqIaE05OKXPCRw%2Bhfn%2B52GV7mxQtQ%2FL3BAN19kqgAsPTvtsDgx3v7S0%2Bt%2FRHUIdC6MDmUT0WlA%2BYaT1WjGd5lBDcbw3Ge73FM5fdTBqa91PxxLImrspKT3jB8kpLEqPhNBus1v3jA4gD6jW2NBDMJHxmsgGOqUBwYlIbF8sWF4blHIy05OPvq8PMAHtK3W28%2FaVQRsNmdR4CKJTTRXXQJ9hcdBN482sCDL0OIxTwvUoLzD4BJnO5CsbGIZgGlJyxsU7NWrjVbO200BWfn4FLPGzjmPp%2BogGTcWEkxKZ%2FXVxpCIsV%2FUy24ZqufYwNTVVYqEfx%2FPwmusvSQ2TF7nrzCeYERJofIHFzqylhmXz8Jcth6hgb%2B1XjtVNz%2FE%2F&X-Amz-Signature=d2db40938fa9b047ffe6a1f84d41f8f9bc194285d0e102afd3e17c5c41b1beb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

