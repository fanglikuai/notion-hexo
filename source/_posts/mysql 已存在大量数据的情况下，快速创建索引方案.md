---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656PNBO32%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJGMEQCIBK%2BL6MMsFrWDsvCO1JDB%2FLPh%2FURdebWCmP5e4sT1L7EAiB3kV9%2BAYLnVM19h07waZ0UUUQMtPjhKQWPWho8GGpmhyqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyqC4rLvERAL4KuziKtwDoHWqPdvrEJNBnNBmJ0GPDLz0nafVJCqmCYae5nR%2FTcQVb%2F0YyL9570VgVGGIZX1V1KG3nZnu3i1pRKvFkmuuOkuaPEvEzitUNp5kiapSlaseRCHwBSy6yDCXpW9daea5%2F5n%2Bf4l0xypgp7a6NG7cag32Olb%2FQ9AdpC5PtgAoQtCTfKgce6HCdRKPnyvHjUTbytHBr4zAU8clrXIUzw7RwEs3z12gumBXLMZlPVQkPOx7OEujBRYiwlQA6NrL6xHLmcZg5bufcCWxa%2FU3L73Vmvy6kTuChBsOt2PsAqybqbKqN43zjn2YX6Mlz0gKyKyMYzyYWLr72h20aVmFztXFUM6KWh1vfhtlXgsM2baVKGJBgOJ32vcq73O6SdUSKgeyUkRQLyahS%2FcqbmnUgSWU0MKM1JoEqdbFZiLmp2Vc%2Fvg0xfQj%2BxduqM3O8Vdk1ky36RLMa5IP1IEbaOeBf888FvU1KKyCAz1%2B8YrX3vwzSzL0xCclGj6jGhGCSrCvOb2i1M61NyUoyOSnWdcB%2FR%2Be8dyKI3Gins%2BqCoSBy2PsOP%2B37z%2BK2mssXVwVkINKnTfLd7p%2BBnH%2F%2FRkg1al8ij79N5f8%2BKAZvEfEiX3vSdtXSRJy5m8c%2B8odVXun7Ncwu%2BLoxgY6pgH%2FtY%2B6wxthb0h94QjGZ8Q9in8tQIslm%2Fv51HZZEs%2BXpqCu5iDkRRa8hPxHF4%2FqVdTWaaQePxYc0WqHmztMfD3BzqHkS8NKMXE8eX%2BKhMKUnuVntb%2B%2BVUwK8u7AOlFIHh5PKf93ZD7e87tou1Av6HSxBBZLkZmPCry%2Fhpcc24LCy9pKn9TKROc6J%2BNj4MUHBC6G29lrvAkCEGFx9TBBtnTaNhJ3gjM8&X-Amz-Signature=1a7bd8a0062f64944328dbf7b2dd853560abb55945d93d1e217ef82021792752&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

