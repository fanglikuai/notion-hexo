---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJOM65SU%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDr4rNWUln5%2BjhrUN8duKhAejUfQeE76XVTjeS38NqraAiEAk9bzAwMzunHS30CApIuzWR8tF3zBYwqvPFI3J0To03wq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDCL0GYSKOiIaCAWNXCrcA%2BhMqXY5hCF%2BrwBtk%2F%2Fga%2BJ8dshfGLeb25S8TFilhr72bJO5AhDgubUU8tvjAsUvRPnqpuF9miVlAnVGA6tA6BASS7qTSQTNENR8RnDYsuRz1GSgCp6KvtbwbWFmaJ3%2FsSECp5UHlVCmjmXKlQ6NkoxRgKi37InC5eF4PTXlDI1AD90RESem5jiD6i3dbzrSr8mHNR%2Bk6QC7AgPhYn9yxFLhtB7EdHHcOGoAUcILYM8EnFQIQV1Wi75w%2FBhWCMO4yTmwzq6Q8XGBN5SQKRPsTlzw1cdN%2BLcZ5pY642rIERQ7elsn%2BdHYwTo7Oz0THps1E0zGGQ5EURfqysOniNtEg2JsApoZyCr7hw5xE%2By1W8maYB%2FnXfuVTfQqvwcA8epBto73I0n1F6r0O%2BfpyKeNHvZ1wD0dq3MgK8BHmYHAX4ZrQyRcu%2F4dDONUoB%2FYCAZDRDrkRQQmjYmpKYSdx7igQ0WVzndKzojZBjPVfCtV2VkHsbQnupe8T3uj4e%2B2JtV8VIrbwHQ2oPg1OrP8QJ8LYPI5ybCYVXlfwHsqOBEkS%2B%2BsCKG6BprPLVNi1%2BT3yVbF5xxns9GcEqLdDZB%2BxdvfHwBMTCrL4ZRTz1Ct9OlcKOr3SMOCNjURv%2B8O0r%2FwMJvXl8kGOqUBsWbwgTnBIr93GcGHmFTfm%2Bnmx4Oh06Hzif0IHmg2hmL2tQxpmERG5FmwnnD5po9wvhgOQ9PmXEitcYqgzRG5y%2BXNmQ75HDKKK4%2F%2BATJCUR7KWxudUvkFRXgyGho%2Fig6P%2FZH26mCCIMdTtVQS8RLtP5xYujGmdF1CbmXnKL3AJVeX3Rv3M3h%2F%2BJmsixKHyqtOfxGAIkHXywiOVfCTpBMj2oGgn985&X-Amz-Signature=170e902039a751949b37cedf677cfab4da74501bc5e6ab7abfb90e3174360be5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

