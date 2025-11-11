---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AUCAKC2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDfCDLG1leuukqA1mCQbRejXYhl1aicE%2F8dm9GwA8DLgAIgZNG4FjLIB9KqJKi7y6G5mp0LoZc3TZ45tH2mgC4WtSUq%2FwMIHhAAGgw2Mzc0MjMxODM4MDUiDLJImjtO3A3wJyaecCrcA9O442gtV%2BZNtwE6cWzAF6iiSV78%2F18mbUceEnRV9muw5eqNLgaaNqpMpy2S%2FZF4TbmnYCqQ1YtOwoTgW0IaCTijHIK5A2G1TXXXdzViHFG5OKPTIU8GnLAe9vgmqxweWjFN4k4xnBgfO0oE8WNe6FC%2B5FGNoDAbtZUupmbvxig55gRXeVcw4HpW%2FIrS8tEwAZYwhWGeEdnMKsj%2F6aE0doc31VgBKXwhtrE0FSS7iDRK8Y7y0EEq0kcQh8MWTWUprajZb2Hn0jcNkMD8pcBhYtlX5qqzC6CipbPjxWqLHIpL66CvXQJ%2F5tjJhMkVCB7lMfn1DSMHJ6o%2FcstK5%2BgDkbhT5HZYUDXwKqgBrj60tPXaMbccm0NgjYD2ZoagTKmhvfaF1eahJFviy2xkCegx%2BXsioZX3vw1F5no7G2yCZdDNbSC%2B9ryKwnjSgPj%2FgSSPmGhxHqw84U6Bj%2FIBAtAbXAdjeR6MMP1LWYR3DvsI%2FHxMtQSa2rBqWSwsD4cuBPcQuWNhASsEY5fHOidHIG%2FP%2FWgtOhoMvpJbsYebnwO4p841zqIuflqqXoAwx7d7f9tAtfHEH8kH0UAR%2BhSLpxlSL1UDSUSJRTfemQqhlI%2Buf4njPb7Gb4NBkrArBjaFMI7XzMgGOqUBYHUQU9zIVe6UJp%2BQ6MG2qqs2GqUVm0Gq54ewKc66pFTR%2B7SUeSZPyfIl4sfVJyonWOCj7m7WHYS8p8KLBImmY8QhIHmJ2Bzhh%2FngUU%2FxewY6eSLP%2BoTgbjOLeTZR8Bq2ev%2FKfbdCnKzG4hZ3LlRKbIcv41iEJkmHL%2FBYMWrse0osdPh%2F2zLx%2BLHYAexPgySvH5xp%2FpUXwoZsKSA6%2BpuMY7Pg8gnq&X-Amz-Signature=3b229c3fc41da17c9489f8a42d18a5c4b658724bb4d1c59ac1d8ce88f2d11935&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

