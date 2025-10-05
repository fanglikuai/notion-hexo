---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZJUOTN%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T000036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvQQ4o8N805BqL1GhU2LrvMd%2FPt1Jo0Ry4sMUKp%2FgD0gIgSgmWlE9fNVn%2FcgO7iz27ExB6wRjQcqektJuGlPO1fskq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FfwBggwz%2B7GgKfEyrcA29E08SnPCJYDc5SHn%2FwyBczUxRBfzst%2Bz4BC6KgTrn%2B8nR3J5CiUEy3BSJD8Fb0Z4HMIh4w0aQi7SxZNej1dYOWvVlHoYt1aGMTH2r%2FNdAKJreYI9DfkeA6IJLROF1LlzpYQGtT6%2Fln7JxXRFNi1cePDGnMG3NOeAlot9%2BcNRifEOEh1NiZJ6E0b8HSo42ygbm7zPfZUKuehRyc8WlPnjHGASJ8gvg5UoXFrGYZjUGTWNGVHsJFcf5rN7vk4US3nl4RRGcwxVbVUBX1Esw01dkZovgVcn1%2BHsGezRXKqRFnGyl%2F45CKYmoquS3Eh6o1R9IdwwwH6Tn2%2BbK8CQBoATk6DsXq4%2FG2WAUX22GYSq%2B3eGjBlfLoO3rxbBo5uJQG9hUb2oEUQ4AHaoKmeJ48lSTnybta4R50niNokL3mO0bNCWjGJJWhXmnfYYNeyXRPdVTV3%2Fu4dLuMlfuj00fwPB%2FczkRJZnJ3ogxi%2FgpfmtwcMsDoyxW5i86BjjJgDbOTolCERRlJ1A%2Bh5VTJYaaeKs9h07b3Iwb5QucbSsNetRmeh4YO8d3rO%2FDPfsbcodDoE7%2FZm5I6G2pQEDbqeiJuo9v%2FCM%2FNw3LsqM5bBm%2Bp1uFrcWz8wbqVLgCuDIgyMK%2FhhscGOqUBH0qTMoNCJLt4%2B22qfiwIaj0ZBV4B3m2VfS4BLeB3ttzrIgCYSCXV7va64H3hPX%2Baudt5URdbtLHczf%2Bma2%2B%2FaQNruPHBBrbEVx1Y62RwBi%2BDFoEIoOuv%2BiSOF%2FjkhymXfaUbGvz%2B1oGyvvLG434CKKdeNFAn9HtSVKhAyulpaNX1vAjSpwOfPpI4zs8qXgyewImxYpmdx%2FVORqy9sYHIvZAvN0iF&X-Amz-Signature=0f1695d389aa2ff11b87089d47a37f9e3807c42d899004b62a124abbd7271691&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

