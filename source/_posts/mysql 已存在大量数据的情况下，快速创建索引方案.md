---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFYUMYAX%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIBBO%2FFv3zMk77UHdzkRZeW6M56xe%2FAatPCKZsOnSUbE0AiEA75fDY9k5T%2FeaQ77a6sDrHJAyQXgxpAlV9Yqzljm3IJcqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDALCzlI0MsHvNWKtkSrcA623FaqXZzG5PS2zDBoYyyS9DHrPwh4ZcOT8vm0TU5cDYWNwGUu04v0rIqsPmsu8QXJhxUhyjaX0HrZD6vNiafJokM5KPjXfZFsD26TbMIXACtLbyl4naixlJA5MSDRxYRWLYObfBN5aPl0zSH7Ly%2BVsl5clkuPsF2s5LEgRFLuYy4BJ3pOqDm3M%2Bau%2BgxikqsKvfWSoQM4mQl90ct6w8P9zerqmjXJj0OOsbevkZQ2b27AowqbjdkOuMA4jsiRMMgEAUR4ifv4k%2BmC7ycwcJa0a7DTxGWELQADActXfGHkF1pT44Na4hK79ijBgvXzKKce4rorhHTEqhuoyws9d88V5EgfijSVKGxk7PZLW8Yj3lOOeSdNPOUyvcFcUDjUJFyi%2BntIkabjv5V%2FSYlgG0SzHtiPQzBAfj%2BlT1Ik1HQFL1aLQhjvcH5FDvQeGUaV8T3s4gcJ1xfuCUU36c8ZDRMK%2FDZvFHj0xhXP6XeWblsABIRlieDeOrnrVkXPKkKAfW2e5tuko2tLtgWH3Y8HB%2BOuoK6ml7XR73LZr1z77PzP2H8ppWvGyT0U20r26golcntYh6yQNmZFM%2BKKZ2xoRmjUircV3peC%2BxNhqq0snJBPdVzTBp9ChgXLmFzfDMP2Cp8YGOqUBgF6qu%2FHKBEmuR1Q8IqKGxt3k7vwXa80ca1mlkBVjs2hV66O4Eyb1ysRuUjjvE9R0DWnOXutdbMuqCg8E0ZXiz%2BEdYJ34G0qyC9tWpmww2mT78jLOvJBgETmVJ52yHDJcNJgJRz6%2Bsibs39q09BT8VUjo1zO3hmW4HbmPJdvnVScHX%2FWmz7pxW%2F4pmJSSE21TtFS66snvW0ZDrhtKqwR%2Bovah9MU1&X-Amz-Signature=5ff96bd78e581af4b21792b057acf3863197b9095cceb5130cb4cce2393fc0c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

