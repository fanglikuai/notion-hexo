---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYQRJLH%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQD3QIhCuuXPidDDXI6OWPA%2BfaoQ9D18PruPJRukvESVoQIgOoXrxEZ8S6vQvRqWP1Jn1N6boGKP0m4SvbTYObadmbUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJpRo0L%2B5SCzESGqyrcAxDXSOdjvSc1s7yruYKS5Q11d0E6Ubi7Aj0qGabJ9D2EpvM0pWzwcVlTx18O1mD4Q1PE66imADVG52tsB%2B4JqLOl5xt1vihTYPll2CfZQIKY%2BQcz6L4ScNrCLfL2F%2Fbq0TdKRBdZQPoeJ0GCoztyGoK07oXo5e4B9OBfaBwPM93EaJEATeSKBMrd29Q7mY2UxDLr8Br%2FLq5nS1e%2FARCasdJ1J7tNPpJhgFkmYS6TeKAlsFj%2BeXI9XcjSDlEzRAjBua1kTaKmS%2BvzZjNgR0Y6%2B4zHjcidHVxfwDv81clkx5Wzzlk%2F2D7X%2FLP7kzVaXo%2B1pLxRE6tKvkL1WX30x%2B9Rl0HSzGWndiAYT%2F7%2BfKmpbrd4xUzyIIbx2Lnyi1znInZZEyIcQ6x8L7fsqLIu3q4jpsaRsiODnb63pFOmtF3Lzcs9wuECvIQ5095sY1UxpcOaZBx%2Fj1GD4s68NUaRkoyBxLsRHf5IrPSUv%2B4Q9ph%2F467ykNwE9gEUbZD7GCA9l3sTJufWvK34HKFstmEK0BYx4c5vYbGxf%2FcEjwFCdifY9GYeWgsukxbCPY7%2BZsAGqJs%2BvoLVlvUGYUW0xsrjZWkF3lWd%2FVhrDOJ%2FTPY8J7YQaqBO6R5pq1FJ3MdmtMAhMN%2BcjsgGOqUBld0%2BwNLw17Tc1BYFrnOV9BRrUkxuxK%2FQQDAVY2MAjqRRhOyRehO7rsGuuAPCMGdiwn8kxc46ZAqOt3z1w97mWRFX4P%2BmgSslrz6y0%2F97U%2BxOvrXIzyeEiZMuEYNvys760mFhJoiU6rL7RgFLDAYUlOWyuTE6bOxJs8WEE6w6cEzVtVlZ1AshBgNvobHjnvw%2FnYY7gQy1w793Fo4jzZWMN%2BJMLyP7&X-Amz-Signature=79b8b3bf3584fa619398232a5cfc75735adf7bac8b54ea813435736b3234b4a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

