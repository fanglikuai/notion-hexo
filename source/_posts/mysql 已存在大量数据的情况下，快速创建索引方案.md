---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNF6T3TQ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQCuBm9aq%2Blx1Qx7vWnR7sYaHpxBH8%2Fl5Yl0gWi0yLvG5QIgKAZMbGgG4Ko1JN5xF8byTpt5xI0PB1D96zXdAzFCJ2kqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAaSIFh%2FnWNnbqeSryrcA23yQDsmNhqHIK%2BmZo7oNfdrIpD42pW%2B9EJ%2FDXT6AoxH3tEDR3y4%2F8IO4xSBWLumJlEDap8cAhLq%2FfR6zwvyroNOYbiBZfiMhiP4cmaV041iqSzhMOhQRKq5IjospEFY7MA2HXE7dHL84WeKItekjwCF7dvPB%2Bh59fUcdlRkvd3dQyzkwop30Fwr1df6zTUj02euhL7lJ73cceHnFdLs%2BeiwKX2GpTxe8Bpy%2BtBz70C0Wa9x72ShHu5KEo3Bh1V6UVd9c4KCwmg6jG8HdSNoXZMgS%2FfYpT0vdGTlgm462rpdfxvT9oWAv6ddPSnpnz%2Bq5jP9Dk9a%2BZe%2FIStN%2BrjN3o2bizOuZVx5tofsTIn4q2x0bf3CCyt%2BhtkcOQGeMLYGIfocUMCSU20qjbbPA1r2YOs1Y6xJOfo5w%2FXvQfAv9Rr1ihuB2JxUsbAdH%2FmKng1XNQwCdVu%2BhIHJ%2F061vSYxf0wNhMKm19jMNHSuqmgZ7u%2FNIX5bA8m74OV1dt3DA%2F2prr1ptB44n8j9VWTipwbQiLeWk0Eu5uy8%2FIBg6JyjUwX0S2Gr5ffTb85%2Fctg2%2FvMPz4avEQSf8tKQB29ahDixxtLJQIA%2B5UJRdC%2B2cXB%2FAXllCd8K6Rz0Qn%2BGqhpDMOKyoccGOqUBDnI6zsKonwFAct2LjsmnkCWLggNSEZ7m05a0Vaxshh4wd6Do7arlxTnepIyidGYykhFgCpz0xJegaXKjc55lnbkgwHVM01H68Uch85fo8ldQurmvs0pYRJz8OGU%2FCHYmG1iOiOLu89DCsAd6T%2F3DFh7nPxAP7lgE5CPcWWT5cwksaFm1wpCqXWdW%2FgLCqLFmUG4p84ZCLTGHlHovedM9RDSkQ8KR&X-Amz-Signature=077180a700135e8cc59ca214b2a7cbb5d62e6a79c88ef598f1870beed493bad4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

