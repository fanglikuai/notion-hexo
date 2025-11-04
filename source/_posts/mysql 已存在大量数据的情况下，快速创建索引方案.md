---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GAWDISB%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGX981WJGmgaxQizrYeP8AvhMrjVPTGAfHpB3jeEd8eQAiEA8D003GW5Yr1mQpeXPpPaJmLjIuBKxtoOozojGAtNYe4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKaoTaBw6qF1djd8JyrcA%2BkmXkYS%2BvNMnNMli%2BwtCE66zhEFOYG4RoJxZS%2BAbzCuJqITcggMbEAuIAr4ZivYmpBQuGLTtTs1VU1BCrcMTqFJTdzrYRc6gQuRNXeZt%2BWOdb4O%2By3fn2I0a7DnNbcoCXD4CuVzKFJYXPRouo7FKWoMQ33fqGkLdys4%2BXJ44wf%2FQdyXguUmPAcpXtdlneAfw4mI9ScsGlAY1K6al%2FlrykVYKkIg911gblpmiPS0lq%2FTGfxh8HJiSGspwJsogxqOoErpvv4hjdR20UR3%2Btms4CcCSAi9W73%2BQrey4OgJaWHcNgxB1kCNZ1u1qis6VX93uEnpqF%2BJAq0QGjWCIAI6DowDoBZOvpDPJU%2BnfkZPwSZwv1ch0fPDYq1Lw6J3CKIzustxTkO5PamJa6z0dB0a1EcJ0sh8Dll50bcASZ0EIwWgRnit0WG7%2FxcgxHn%2FkVVf2MirUh5HYHz0GkB5nXMV%2BQn7Stl4o4sJ5XLOseCO3U9%2BsSRl5PDBaHYUuHFoTZNy40oumz2MaWhBMNAH8LUtFE4dRRrNkA7TYMYftJliKR3BE0yNl4G002uHW3L4e1Tp%2B2J%2BczJJTqJstmIgSf7Eo9KTRjXDIzAPV50WYffnF%2FBij2wZB%2BKLHX1KFzATMPftpsgGOqUBKUIST%2Fcd7EVCCgbc2Oiiiy0R%2Fu2Y2zMJQNiWNdveDusTP%2FfpvF4Fj9gT9rjyxbtmidub6XWhic60mZnKSAnV05ZitNY766RUdoPh84tCOdZTqgLsaf%2FAzGH9jZ6FY8yvhIN7F%2Bo4XJ%2BjPj0ys5j0yPXJrUaiP%2B4OEx7DRUyj0P9HtWMNJ7DBrnZYmZtTfy1C7yB9dGWy8bhgaRjPeadfexs5VZRL&X-Amz-Signature=65e438e269180e499c0d2c0dbc98a2c65fa99f4041935768194506ab25e3dead&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

