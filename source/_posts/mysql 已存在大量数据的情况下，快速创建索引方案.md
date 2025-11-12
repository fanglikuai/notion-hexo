---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJ2IYCSN%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCVKxs0r8hC0ZCukrErO8fVwd0mzGF%2FN3ar%2FnwYveJ7tQIgQlGcwN4X8Fi2jic%2BWcQ2eNW1fxhnlsDPfQtIM7Rdl0gq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDERpWExtJ0bYH8XxlircAyQdjq%2FZ8OXsSIYhyjy0S9K%2BZ69dIoUW%2BRvQt1Slw27Sl7RhOL6GxPXYLiETjX64ixHnVoiBr5aw%2Be25G%2BtGmXST7m6D%2BRGlgct2mxrrFOkINtI5lWdyniOdLfqVi60Rbupkbi1LVguz5EzeY9hBzNFdFv%2B1xZLY4R%2Fv53cGw5vbWP%2FNYGEhbX26JQX33%2BgTOWrx9CCTi39DYgVM9%2BjDqcpDgIdsKQQDRqDTxA5aD6nJcnEFaVUphyQAN7Hmcl6B255s93q37ETItrEPTkuXy1GvdrVli4sKi4xSw3QobBLqDJ3m%2BOLEuUjcyoftzd1sQzaLCrBaqGq2kB1j5bV233ozmkfod8UH31F3wiss9OnodtKSf2im767qsJz1ruayJm%2FjeL9Z0YVJlUKa5dTN6QC2uFE5IsZPaQ5Jl5eo%2F98prNM4fSmhTeSe%2FaotZr%2FT6JmErm88sDMo8i7oLVB58gS2qB5taUrCJR4ca4JiGBf6HIfx6TU3sYE15jkcf7mrMf9F0G67wDO5ChPSGWAeNMrlys9Cgft2rsw7kv51Odwv34RkumWVMfYhXzt%2BH6V1CeDE7BRhGGyJHVFipZVQ3vgGIO8n2%2Fro9jmPeYVbgFGiDa7p%2BC6bX9fwQJXQMJ700MgGOqUBfEDh8t6kTBcwZlYID%2FhvVFYA8Tgz2Qc3dFNcBw%2F7BAddl%2B3ReQ49eydsLOJbUoKE1kGks3IV%2FCR1JkG4peK3%2FdLydH4lie%2B4bSMLc3HD6jSHVwWyI1uiwscO%2FFon40ejiOYfnoeHUQCzuXPqKl7vQNGJFp7bgWMIbNfmqH3jZCOIq7w1Heoc51i%2BzkjWwk7V%2BP8aColYo3h0%2BykzFEyUik69WuQy&X-Amz-Signature=3688ff077228880b8a0af88bab5326cfe299868888afac15411904ecdf23b2f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

