---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ2M3ZLM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDA9Wz8TXS781U8Jiw6IF1XjbmG1CEhk1MPNGn0UGgMeAiEA6CEQGQf%2BnGIkoGbOR3Xxl00O0R2nT4dMi7N0WmP%2BG%2F4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNutSz7psuMh4bUy0CrcAzkqluRfOajj2koG1IPsgibcEcirRcKWVdovmFnRau1Hg9QOueJ6%2Fok61WOD2%2FxKEN%2FDyBeZMO9FF7tokJzCXCvOSoOhrPj%2Bp4yOYiauuQ%2BEHJg9nGNP9IVIGlGhdOBwN%2Br34kGjicC23vZ6YVKIqhIJYuS7giDNInSnAuaLGxzS8tVAhmD5FEaTsgwFI80%2B26BpIltKLFacisBSILjQV%2F5cxWT%2Bw5jVUb3%2FUkNjJQDvj3dSyw4tD1XUeTYWezqLbvUdcIvT4ROPGxDfAtpyIBJ1psyWi%2F93Eikl%2FlWVgiiSR6oEr2TXPTXVNuej%2BclpY5BpyilHM8QAn9U65xSu8r0aJm0cYJ3Bigv7KvF4BMusk1mSSJNM3%2FretonkIZ8zz2tQ1wa1ZJeV%2BMY95ARJ3igwJBQjEmpk9oj%2BS%2FtmgSPf8TqsvVobsdrFhYo%2BpsVWyBqgRD5HVN5nvgC2EyyaSiYs2FMPu5DzM4KYjPA08llx0Cj7dVfGmwiXIXdnpQEVRh39FB4jm24M7b4DrzA96dMzSgFDF3SbNchK18jp%2BQNDcSc5poXJ%2F5m5jRgXFlYtzpIDU1fcr2Ti8%2FpNVZ9fHrd%2BkZDBKE2D64l3sxl%2F4mc0m%2BkeXzdDY1MMfiWCMJeR%2BscGOqUBtgne%2FUjd%2F%2FB4PYVUdxzFEDmoB8k4AbPNgxsnXCX1xpPls3N36Mbq07MR1Zze1igsVYNkptF5honuyyIPBIArIk9ltR9oXJT1ty02Fvu8muq%2FsOgOSm4nwBjRB4tBjOK9bOQKoPvcm1Hmka036qPXcTj7Dy8z6VX3WqJQCaXECu7ZF5HwtdFG35mNTCpRuPX1tuVnF0116OLkiq2139q1yYhUHwhA&X-Amz-Signature=ac6c711d0322822a4495c224afdbd0fb1d2e1cc51e71493078f94a4e9f3af491&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

