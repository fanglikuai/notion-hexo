---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KPFY3BU%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJHMEUCIFljmd2neUbbP1Cz%2BbQjmE5tXNmFQ0KPxgjFS6aywKDMAiEAiIXm42kcwdORicZDsmVljb4HzEkAx0%2Br8PGg6Fu16PUqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKTC35i6QBSDUVBGryrcA9N083XoKXi33iJFbqs%2BIhRBMXRxM928FkA71UOt0ogjL%2F2HLOMcmd8x64%2FzidOXIoVUGp%2F4HSvvBcAjbgg6hm4yjvEPlJZUY94Sw9nMvkfNkvXIIHYcbsypISJHMjxXmsN2HIRQPSt3EDWjkSP8ymvBId4zVVwIdvyCrmgrJT1jy0oOqzsLTRW2cipx6APdiFuJHcwkYH7tho5CqgvAw2h1lhGlNSI503wdhQHBi8EGITWQaYghbRWbAnSWNYcPf6OncRQeHR9xHy8gUmHJASMypSaezlTjV15hWu7A4UOXIP%2B6O5gX2O733OzZmkUkakyahvXKnyWtYhcf1pp0oXftvVqZC%2BvfKyrL1y7jaJuk4VE1uZUN%2Bo%2BigDW%2Bq8In77oMLZnL4z1Ju6TEPl%2FQdvM1JGSNUWnfQ6zIpx94%2F%2FxIeuiZiYNpuZ%2FPssaYnpGdkEBuuT4%2B9ptZnBol2wUiJj8SiaIYvKrfINCOQcEtn3t24e%2FV2J8ceuVirfTxQUxxNQezVmuXeaU2RsRpN1p4%2FdGuNPYrfh8C9yHLwmxMSY28szRdPrBWp3QD2XdfXufyYSr%2Bysqkn%2Bt%2B4iaJmrDKGNoo%2By%2FHKjphEMpdFHqSDH%2FWrJOC%2F4IpFC2%2Fv%2BzFMK73nMcGOqUBmhN7w3phjIxIHHOAIMopSESkTpqRIJ6dqzQ8EA9b2NJzbUPPD85IRJ%2B1ueEtsc%2Frck0ikxMiIoz9ptC%2F1qWU%2F0WpXZLWJFKqCMLDEgq%2FhaF25ieWBFuI6xsjs2fJW7XG1Gwb9xs01F3uuhi5gOxE8JpvvQklMUoknyVSBx8kiHPjwTiEhKHtK6nrhdpAfe7S1UsQDcUhI%2B6MLlU%2BfCLjU%2Fbd4h%2F%2F&X-Amz-Signature=5a6f266e8a111a07831c91daa4718d050217033fd06de22dbab64fbb5141513a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

