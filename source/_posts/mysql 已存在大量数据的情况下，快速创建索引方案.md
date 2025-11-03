---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPM6LT76%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDT%2BJktxrsBmwsF1kc5ysbNO5S0wtDTg6bWUHZDYu1vlAiEA0vyWA9jrkEATmA9dt1oiH74PdQGu8Pfofyk%2BWghe1GEq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDIarGWX1WBvkr2QCJircA7dpfIE9UDDCpYbvbVOyeeEB8%2BKr3LLDjUWkbvfS2kP6drS%2BDYPMgz7IIQmx1rrqVFW7LXdPniPvy8XipkbMdNwiIBSB4cqLhBNsy%2FqvNDyLEhvFCvUnLZMHwXYVtwgTOU8maUajg1zWkq7zMUbgBS7mDpijYNkpJv8VD3fAE4kVkNWPWiZOnxvyMMywBGkmD3F7yC7S2xYOf3NIYW7IlR6xpu1Ad1bfWP3A0tAE2qTqizM2O84%2FP1oWAiPbQgW0uZpLz19HClG47nXsKAtmTXILCOM52bJ2mAoE%2FE5mLY37k8PD%2BlpPOWC%2BEnLnG9x2VzOKysjNcz078mXGZw1naXqu0rOyrODF6OFeYP3ACAjdcb%2BKMx54NGNtw1wKzkOaGQ0IrthzPyseHmIf8%2FLFqQoMaYv9jDlhC6P0w1J701rLkQFt6Sq4a7NCr%2FI3ZlMR8dEpXUuf%2FQ%2BuhhqjElGQPwVuwZx6LKoz5xQRmti8eLBs3RRzeFuyJj5V%2BxetuYznopU%2FQwiDJ3Gac2ihZ0Eg5yMdi80Onlu0Sw3vzUEcVRuFc0ywT9dlgPTDtaTOzoqmaHAQhn8ZSz83ENricBM%2BbZ%2BqBL13QxG5dRbzZcwUpa439%2FG3jP9D5TrODsENMODQocgGOqUBqfFZ%2FAaGBN46fvnT8xT9zpsxOehGE9qvJEKcWgz06SHMg1BkNWvGGj8JNT8g0t0GhWAfo3B1dswI5jxAibmFkiXuCkcGxl6i0%2BhQsNxWlhuJh0Fq5NVxi3gxC3ud5gTv7bTNgJ9eDFpD0K1G0cBtyqlw2hnDixkQsin2EtGVqDSjEyJB8kftHOI2AG3LHkBt2AgxVUJmOEgpEvkV8IW1JiX%2F3cIa&X-Amz-Signature=323db262efaa72e7894050560011191659a372b1314d714ceba644903131e52a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

