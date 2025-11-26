---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKLWWLI5%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBGsc6slF%2FrrJLGoVeczsBQJvupiQhtkJjCN8v3DcEPNAiEAgCR%2BmxdbHGsgMoRO4HCxkA4xREDcdB6NBmbzWkGQ%2FG4qiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABt%2BrydxytPICmZfCrcA60AzxI5anlO2vP866r0JEXded6JqMTc5rpFx3XEmaFfEJm2zGwjpedjm%2B54hbVEkLlIyEVhWUMqB352ADFpadzszkoONcPVjS9mDIH4Fy3CCXs1ZC63E8ybbObSuEZl4%2F8UtzqbeWXdBbmfopN0lA9ktzECukqBnPm0U%2BxQMOiIji%2BBsApKYN2NiC3stE236Hueh5CFFCA3GOAummO5MgvkBF4TUQYGLrGZg6lJ6GlLou4vsaktzynbDqpALTqM0cEIQSs2N%2B9YLx8zcYafgOd39lLEJuFweF9a2F8CZJV8pKwFyutkI6MjVMHtI%2Bj%2BmANYamPcUF0FoJf1hp9OCZMQjWO9G%2Flx6l4O7dmv6VLpFo64YQ%2BUSdO5vPSR5u2E5ucn%2BlacK6kMu2QHd3yefaVLR1SGjqpKfbzwyeOJfKh8Yd8jIJF1gKAq%2Bre1rGfvzDiV%2FpSQvm0ZSEPVG3xz0UalNuza%2BcpSALSB9bUzqhVgVNr6LONuk4NKOX8J0A0QQokSwLoIyfOfmd4iAAdX4vzkXnd2zSxDZVqNMr5m1jUJaIsrZc%2FmoZANwImi4b4W9kJzEWC8a8GoL0l9%2FVCJrBTenqFAYHStXRIUsqn68kyY3CnLWeJb0OVr78NbMM3onckGOqUBkup%2F9Gf6GM41dkEgqt12oHEimcJy7l2ndevyx03gN3vtZ1tt4Ef1uw2xRkXrkTNsTTnhYL2ZsMwPaJRGaGs10qBeK2Od904467p2%2B9z6V6xqzQZvAaHT5AKa7r6INs7xbyIaAHjG52r5KoVolqClezkrI57MCjVoloBt8b78mPhUEbka8I6%2BjNX9ke1dt5fEOVVoJYR72xdJ0JjrXKusagnK0SBj&X-Amz-Signature=be3431e24d2c8de3aaa2ce9eead87cc46656b77adc008a67e395b22f77e5d95c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

