---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4RBBRUZ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T190046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQD6hOwCggeHD5w7NuNthLpNoxs%2BfS0sKhVG3051etKdmAIgDwsc%2BDVUUWU6WkvUdfy9k5%2B8%2FAJheag0FSnWGOtP7s8q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGzHC7JZW%2BPpF2GVwyrcA609%2FIHP2h%2B3heGhDlyQVIEh4lCCGn3AmVMmWDDmZfoB%2B8VEpEiyJay8Oa0eM8i13gI4YmthVPmJEf0lOeb19uGqimFO%2BVtMtR4XeHWi8qcDhV%2FpJbYOYYLfE5F4rYuGlw6scevCmD27QqYuHRIpXBiQu%2BxrpCphTrfR4KGACa6aajp9ezOCwpksVaiIqOvpKkBDWcEKf%2B51pVdwOy5EF1kfZ%2Fa6pC3nclqRJ73iXLcCH9fLTo%2Broq5UTTB6anxvMgxDX4lY9wB4ZdkX1HQPep92ydnXJU7xOqo5VWDw5goIpBusbiZutMrVGndAIkhvrKr616bjxWIERQ8JS0hsVsHvmVb1iMt8%2FYYZqfEdJM3BQ4U3FJMZ7d7%2FLn5opSfz7n0iaqi570EY2GCnkYR1xsCF25OHM9vYWN9juftxILqCbORTEN%2FKccxrmHBi0aMnHyLhDR8yq%2FHxg7eImDueBFAIdxVvzB%2FhORQeiavhg8iA25TTM7VuZ5azR0Vdn4MqQiG6fDRGzhZsP5a0PhB%2FN0RyAt9r2%2B56%2F1HiTZ2HJJeMteU97BS3QPwZd50sE6hA4xD5oM1tEFeRUtKCcYccF66UVEZwnzfBLNv9pVAoihDEpQV7ckrps0XOrNxWMObLyMgGOqUBS9vv9B1ua6penmmEidaw3BqBVwc4VrhqyldZtRkwsURCuWYuOG0RZfB%2FBKXl4lsuIB%2Few4cJuQURkPL67qYSVQW6g47%2B5giawNEUjkhwJWM%2FVVpX6g5SrvBvUs69P%2FY1%2Ftv2259huyzKIZrB4h3XyUaNbE7pYHXmHse6tCqX3ZTSTnd5vWWOJkZo3LxpHIimQdokLOTOYAAI6ArazbQTmsHaptoY&X-Amz-Signature=8f0b5580b5fe8c2e4b6849d8a5363ee51d222430cb77b6d497f7bab3442d4516&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

