---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LUAJDGJ%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB3Ry7BXGu%2BvgvLBd6zp7SiRxsmScw2t8AesT3%2F%2BGeQ7AiEAkJkazj5B3w5MJ2QIYtIZtjfDThQXBPXYPcHOlML8go4qiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL5YtwksG39KUI59SyrcA1RW85TzCNUcuORAq1HOgLb%2Fcu%2Bp9D01DedU9kdS%2BJwxTOQctSPK9nYhqj4UIaGLUIg619%2Fuk1NSrGst9yQWPONLlxGxesthg5kltwBg09SYK7ZQnmxl%2Bcm55S2zLnNGWegnWGCCfLb4aOAYYBx%2FjvUcf2nAZIw3rhNEeQWiPjt67C6hjwqaYZyH7mNOhQHFIT1s34SF51jwrxBhKDO9XRirv7qclRULMyGAz8P%2FKLqkb7sedgmb1%2FduaDSjB0h6ByAC6dlsHqfKW65f48aU75kIsH6mM%2FAoUi5P%2BXNOoD%2BKaycgRutuCxSZv8krdjdEcfwyjLXwBMSxubYPkjse%2Br8OJ2%2FphW27h%2BSD%2FQ%2FPqJjOK5UwRuDyDLffDDeHME8ZKdv6WzR6FATvS7aA2Eael9pNxAZBhjiTduqb3Nf%2FDlym9e4WFQ%2B0K5Ft0Ht%2BXt9dUD7SRk6Cbg3Ok%2B3WXgxJF%2FlakvqW9sYTaWNa%2BOmNESNAibPyZt2YhhhPIbI3TansON82pNwlLA5HVlX2mF%2Fvxo6c4bhoK9H8pSIp58KcvSezW4azqZrU9yrXK2WwCLwSAWR5VCoqbE2%2FkvoFv6UYvLYiN1rAGu0UL%2FzZ2%2F8s39cl3kcKYxQNRoywbklyMNOqrMgGOqUBYYcqnpDMvz5AZykFnTuINt2xgYpPoDiVrQbFf6wzr8Z23vh3gGQyL6YHfYHRaaNiDYjT%2FMdD%2FeZxZ1JR0qJEDIEHfCwAMMxB8Yrh7Xfe5H38avMJ%2FensyQPCk8iUuNiMpss7KNTV2U2QmW1ayCdwIhf3ikZq5AytYWO3vIRHOO%2F0qVX1X3hY7D119Xj4gPiJRztb8akch49598rsqhlbpty0fduA&X-Amz-Signature=ad3fd73165376c8955d6bbf83dd85ffaa9ea9ad6bc38000809196c8f373d5626&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

