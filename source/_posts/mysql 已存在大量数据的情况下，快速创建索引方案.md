---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVI63XWM%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEyvupk1v8RVL7nteY1l2xe7%2BEY%2B%2BHG%2FpMY4FIHoUOryAiBpfCY%2BfihUhyZMm8ayctmX4SnlSla6bsQqc9WkD%2FDRUyqIBAid%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaxQRgVSAbeUWpNjyKtwD%2FM%2B6AUJv17ieGCf8Ldvuj2fG%2BijoEQs2ZNg2HQMFglhyKwODmPSVUoWJQ%2FasbAjTYr1V3S7t90By%2FPab6OyYaD1gwhKTTH34Jq6ddc%2FLPB0kYSxNfN0qisI%2BvzZxOrI59y%2FRbFWW77KLOIdINeDV5InCRvjIpq%2Fx4k0k3%2BSmDPGwlHA79qC73wQ7oGbP62ja%2BV3duG3v09kzRt94mn%2BmJvkF37HqDbbuyyGZTIIJ5X2VXKUBPIzGuz%2BGp27op1xmoA6sZyV3BJM7hL48pwm9Hex3NIZuxwzq7BkoydwjImDAvaakr0Yhm9Bot8BdLw3dgTePejEKeo8J%2FBGCtt%2FMyXXDnTVxSScyPUxD67V8vs4HFbgGmJpHeXZy0c45Ifkl3jQcIs7%2FdHl27SzXbchS9HDMIZ%2FI%2FRmbF%2Bo04RZM%2FkkkvJLF1fNYhEhpTqQ8wduU1vcVF1GRL%2FMcYwenMiP%2FN28l8eE%2FuJWrpIgL7%2BAJ1lSqhFCuZO3w58mJG510Zx3MupPD1Rq%2B1OLTJZ2NBO3vHjLZE21frJR%2FxMvAkThbPxxJJnW2oxdgdV8jWO3xiGh%2BzJB2Uv93R56BTD0%2BpZ3SCR3p09YYKmCxnwS4vEr5eSEd5b3cBDveTZl356ww5837xwY6pgGx3gDCD%2F0%2F4T9uIT5KD%2FViP%2Bf1xklt76Sxcbi94IoODBrfo5pNPFTQPMo7pEGE3G3d5aBWzsttAYYY0qRGv9uoZftVQbsI71SfdrUSWCtqR6%2Fqrj8qqOlzCPLTS7oJeo%2Bh4%2Ft1Nyomm3gTMEJUtqVqNkODWBliNE7dZ73%2BfOHsrWY67ILDLgLJW9rC4u0%2BALKuhpiL79mAGKGt99F0wM8FHVT%2BRzxC&X-Amz-Signature=b0f71be8f996fd03c2b09fc0db21ff3e16f2eab6c382145e680992adfcd3a598&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

