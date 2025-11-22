---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPAAFERV%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIFtGmvv%2FaM6pnNaS5hw%2FUVv5xajRjqlulXmZqnwCCA01AiEAtWAXgmyc2z4VPIgfqP5m%2F9X4oE2uFxKFHcNZ7iUecMYq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDJXq1H%2Bh5w7AhYbJwircA6JCpv6yEx8kWSw1u0CPzVbEEOs98NRMQKh8%2FmI%2B%2F2coBqx7aU8o8pAlvUyVSn0GfK49%2BRGu353U8Vx8MFUNNlyi6SYe7DCO43K0G6IKQMkF2K32a8H26hiSUfTHdU0ty7JuCG4uAVNheKnN9KLaB5wXCGrVkKuWwoNROqLVCSSuutfp9MluMy6Q5Fa%2BiwjU2SRduREMtAw0g9MiOJGJml2YCllKPwGsOWBTh%2BbUgLS%2F%2FbxmyBIdjVnajesgFWAet0AGNn7ckO34ZUVg1O6PGUDqdTTjeIONlu0UhkZclwsO4TcorhMXB2lM%2Fi9euFqhTX4ChNjzR%2FN3K1MaHPQ1EYAyg4dFESkDwktz3%2BD21MtoPcvgaDGZtDebkrhfnNKwJh2ThjR%2BHnoHHIEeLjXz7xfDUjIl3fiVS%2FOrYN%2FUG3XlPhDuEByHNX8Ep1o3KTXqe52s1OIb4fZB21Z5l3nXwGNXT5tq6eSapBAO5%2B%2BEYD0%2F7u%2FN8FlecAOavMNa3cagDag5Ft%2FUTUHbfa4avja7Bo%2B%2BB6lmLHz1UA11L2W4oae8fQryiyYMgeH4Wjhkyhwz51xDoXQPNdgFjTKOW73zvEzGNUJ678hKGPzMji9knJoH34YQrd%2BaFWw5NWB8MKDmhMkGOqUBvqzdL9U7m9vyqnB%2FYo84CN35X5brU%2B%2Fo1O8tzUZi2tGCpSXYdlztK8RZYyjInJMT7gvTpV3YP6CNFG9yq%2FkZ1%2FlfsdjR8nQYLIH3WGVQrNy98vnJfz0rKa0X2OYxs1u6gW28a7V9%2FX%2FXM7WEHfhe9HvmucxB0HqPRZENQxbmwEtqp5Evv51FiM7bznrp%2Fe%2FM%2BQKzWQcWF%2FFI4Rf1CpazWeVPslnm&X-Amz-Signature=18f33ddb414d0f1709b06ecb289cabf37e6857bfe82218bf459075ca776148dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

