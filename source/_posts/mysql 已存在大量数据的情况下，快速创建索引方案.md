---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYZZBE2U%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDorLTKu8oMQeN3LDhfA79K5IugEN9gnG7Y7v4qEmnvhAIgNrcF06huSbHB%2FboIfMSs%2FN3zj%2BauapWfLh3b5Rh5IsAqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOKplGqnq%2FT0oYsO3yrcA%2BrXnEDl5l%2BDXQLmwQrcwBNA0xQSuSIGWzGaAaWHiThCJOi1ypYzP7cQutJlCLe%2BqKLkDkG%2BaYpmojqGhEqBBIc2j19a4G4tkTC7nyiu8FT6q6jCYJdid5Ow5UawtxGwZUy%2BlHJ%2BwzTlOfczgvHyaEAZEUKMAOI3Zpx%2F1jVZ1ov5bV8a3UEMLh8xj1IdGqdc1Bmpug1qcA2X2e9HrZ5NybrogrWbXL8Ykz1hdGpFzPOAh8AGbJexukP%2BBQN58W4xajrLMrvQctwaqMDYL48p4vRBWqaMkK%2BTTbe3GvK9hvrDeBo44pnqddWD9ukLGe4ncmyPx6S1nOXntp2ZnmUWml3iLVYq5W66O8OD%2B5RucAOqmNY1THAHLdV0qrPiNzZXIxaaUHc3wZP8low3PRwT7cy7tAC2hc%2FZvZoIypfZYCl3pmkye5ecG1X836I9v%2BceRPy5YwrnQvL6sZ7MdrhfC70eX4x%2F3NzUXsxhj0Mx%2BTUdG%2FRo37FBu54xT%2F%2B5FdpnURNhtExGDJ9c5OsYykiU7lhKw1wXXFKfOiyTZ9MvxzCfehbKVJIvx4pkFDWDrUk8caojOVUZXPjYpRQ73r9KGOI09rMJCnDT9tTue9t0RTzkWtMvXOz5nz%2FsBls8MLWgssYGOqUBbzAcpgexknIerQs70q0qURoRqc%2F0Id8yMUSQFGbboIH38%2FylUwtj9uSJGiGpPF4ziv27cZIWBmGEJy75w7UQSV1fM44ca7W0QbbQSHHuZW200%2F%2Bb%2BKAXf7NPWwDV7y6LC24uxs8prA8Bc8J%2F5FDxXVlap6hhyEegUrQpJ%2FVmVZbhXzmp5Wwo1B4%2FibDDTngmOoWkxswOLXHbCsKm%2BYlVCFehRxa%2F&X-Amz-Signature=fcf735418f9366efea57b2791eee76528eb7555ef785d6eae57bbe39850735d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

