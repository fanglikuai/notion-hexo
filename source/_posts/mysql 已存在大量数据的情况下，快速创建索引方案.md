---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QW62VZDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIA9OJR88xrsHQ7%2FEPkqv5NigpoeJJ5yu0ikG7rJGAr2SAiBDPBmFVi0HyS6fJBtyM5VuOJiVkQMs7rlujnaRD4tspCqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJXqtklNOMCiLioLBKtwDkfx7BTMvJdHZZTYngSdMMfq7C65uB5IRtalbTsZpdCEoauySXbv8chj1ZaW8ckLJotf8qWfonP9Qk9Io%2Ft%2Ba%2BVcV1bdJDo9HjeXDmXwN0odIjR4mir1DWOAXn9D0JR%2B5d5MQ%2BTPfgVtuCLkl%2BjtTmfev7ZF1xjjeOrDQvs1OsONNpoHtoA%2Fbh42hj80uKiPbSRHY%2FPx%2BVN%2FFJJCcQzuEoNItNXK2W%2FyT8iAL89PgVA4dRotMOu%2FVmWLIOwqzRFZ09ig1K95T1XzmvVbY4LatKiVWGvCju%2BcZpiGnj6VTaf8fqCtRJdDZGoypAMlZd4O6A7mMEbVDObyklK5szPcCTdqJqjdiR7gxYGAo7KROoldYz3Kp9NxN2bAN5ssW3vl9op%2B27KR5yWbApUzw%2F6kQbyoGptskpxN%2BEIY%2BBGa%2FqNmqdD55Ejfrqb5OntNb0iKA2cTfy2b4Pbfik8qHCdYJIm%2FspBzmgHSvLfuE4jlKgVyb3Yeqft9VCAwIb9veQ760STe1N9DBZdFpXuSnq9m%2Bs2An5pjdp2bl5L0%2F2S5zLoW5XlmrlXx3Jj96bALRsBydh8xSdVDyKNvL%2F%2BcguZinmC%2BVuECHgQrP9%2BShifcpJOm04v4DTLrI8X4Z4Qgw94SnxwY6pgFKPMtoiISfZunNhTAIYFzci23W8puUsbkL1hxiaUdR%2BYPA4bxH2qhpnwPWKpKY%2BIUImhxj48q%2BhFcnjL8aEIz0ZoiOV9FvyDU5mT7BPS%2F4D89mGfm4LpFdNGMRrSB4sIcUOTiucq3GA99VV%2FY%2FJbGfzLmKewxFkqnklLg475QVUvph7JVK44JFKrO2vvOlRDsBD5V1kYYfissfwerY8Bqvl2DQ2f3d&X-Amz-Signature=8f881172af0839f15f478da0898ea97011f53f501534b08ae882ba9534dee608&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

