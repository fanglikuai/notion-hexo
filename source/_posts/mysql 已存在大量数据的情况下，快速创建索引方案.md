---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653T2LTAU%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIGJwCzzX7bK2lbPExOEgdOtWgXiPD2t4kKRR1ECaps%2FfAiEAr%2BEp9veYktxabdkt56MHsdvnb7kCO4IxXrf6LhXNQGsqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBSGIsfzVL6%2F%2B1DVSyrcA9gc8V7voJ2Z8nE1MmQI%2FKr2us5pvURxqmRHBGDNlUuB3RPnNgRO%2FZXeQKduCMyQNtHFCjYalBPs02R3tuuDNIzwHOcqgGnLUUvwhm3h23EOJsmD3YirrEPMRMhb1MspPvR4uOx7KZ6iDFNhtqwNUfgFZpibw3KVQxrY6D2Tu%2B6Lg6RMb02011oPEXNsrYoGMZeB6g7Qo1L9BTJtMQYWk0Np2XEtkxE4aeXfN1ZGXiQPsahtlPe7M8kQGHMueSzRz8WtO7JxxWrG%2FRzvVC420J1A6tf%2Finf86E38CXiiaKNTWUhgB8em2j8b%2FUUtoYfuu755At7anJixjNaq63dbkVbSIVCCSsf5YedOwZwqmGUgrrS9mQXuB8f6Ocqjbf8IPlDLnUk7OUACfAUWR6ljGCQgiCNb%2BX1dfEw2hhxziixX7X2ndJh9dw%2ByVxYn%2FouC1DWVLEYlsiQAhRqUYyEdFu1J3phFNChZMlksbvuMC%2BwxlO1oKGc3q8vx2Rjk5diT4JI%2BheyHO%2BqBojhqj0mSKRmCH1BWNsXOV2dej3k4%2B%2FAIigSOeUdo4%2F2IHegeR2xpuBDFFw0P93L%2BkYmSZn3lpbT79KERJL56PUbkPSzcYCurfHO8YaZMDEjiDA%2F5MO%2Bmv8gGOqUBg0ZEgmI3KmlTu4PBUetUU9ajhmPluPUFIcEe1olyeb4Z1jvGYPJXNr6qiDuGSJ7xhVSSNplX2Qa7USVEc%2FLNyW72NB8jK63DcSTWzCcP2qzHC0nKJYPk4fReXRvlm04WDR%2Fgfxj%2FpPCyCf7PDJ7LpTsC35zgVsGPogBBafTTsfAHy0oZijHQrGUoXAtlnyNNetkaOLMKVaXA0Bc8LUuX3Tfk8nPy&X-Amz-Signature=d43abec4230ef347455f3698f036babbcdca2c04fa93c28ec33cbeb689c6729b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

