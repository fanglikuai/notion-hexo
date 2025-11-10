---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM5XL6SG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQCOujSzxtEUyDhsQ6vs5%2BxikXEia3wsejTkOdiNCu9bGAIgauj4dAe31bn2p2xWtohsecbuMwnfwRqTmpyEw0ItHG8q%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDIG4DQ68s9LLFkVUWircA2j2fOP0aQaxGZPU1jUBnTAGH%2F%2FZWoE%2Bt7w2c6vz%2BdG%2BXpt5zS7elTbQ9WZY8jPkmLkDcCh7njoqASNqRN70eV%2B7xj0cRZbimT0BYimUAYdE6JP5fC6v9WZgJWrW25Gsn5Tu241YO3qzH8WR%2FGQVNuD78je4FhW5xPKqXXN9anv21ithkedst48kVUxg7dfXKI%2Buw4eH%2F58%2FkvCMoWd6IL2Afb0s2Bevg5sBmGIIwXfgD15hjv%2FdUL6L%2BVhqaOAFDfrLF1dH%2FlH8OdzzC8Dvivkwizc%2Bv%2Fu%2FksrtErslAnuaOCzFaz3cnFE0grYD06wYLBvt4%2B9CEGagJrWFuIGyOUfFigQqBH12ZiX2659mkWqyMcC3J2M%2F9kiGpJaes0aCri1eodgvunx7NIcXVRbcMPo6nAJ4ow%2B7JY05MaiRphYbCI2SluBisf7U4z0bDq9PxM6cxYN%2FBUtSMpnw6FNhnRtNFAxdFDwlMLhI9UaZVgms2OLpYLcy8ojJ2wylX%2BbCuRvy4Zx5PlXWlV%2Bvy3%2FjI4CbN4G4r0TY7ZBNXB%2BgE3SSbDaGHyO5xqv8ScvwsrFvsFXq0uiL3PqXE8WmtF1ZR8BnXb4Id0wig17Tgy8ha0xzB4U2sCglyP8sUR4pMLGYxsgGOqUBc3lwpWvrnKaBB2a3NAfkZyqoOW4QYbLSfClkB9HaRA0psO3IjOFepxJLlJO9kUey2mEu4ceysmP0t7E6rwDLs4ZaNZjbH%2F%2BCz2mZXCXIE%2FOVDrWWMChgzzISFXvi3m1%2F8Dyp1jdvTlzha9wUSRt4NIuTzRjDt4xnAAPxBWo6SbmxV%2BaVkcpUvdp5%2BFdqbyeFJ%2FbBMqeKswjgRIVDPvmDxbjx%2Fhsk&X-Amz-Signature=4d6495c605cd9baaf1c96f79bec320a945627bd3c287d2a52d63b8dbce67340c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

