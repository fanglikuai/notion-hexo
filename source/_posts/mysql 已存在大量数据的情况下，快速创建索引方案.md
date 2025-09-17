---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R4QFUCH%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIGh93wd4EAyu2NZ%2FuzSAsqOBXiLeQVn4NQrVwDmBIHYwAiEAvEkSbIXHbRAESOwPNAGoIyXBqaawXeG2hUhxp20dgiQqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIXpRCG1Xz62nrPdRircA2esSEc3zhq%2F%2BxJpBZU8CQYl3FNFC%2BaXN0Rrwf6WuAJ2qvDLVGUZoKIsjWv5%2FaTle%2B%2FcDBqbCMc%2FThKQ%2BdReMJrss2wpGLI7it%2FmQKU31zQ9Gso3G%2FBYA4CmrWJcR0GjfbuNSJASrzK81IgadyaTtblkhVXTAhFmFf2jLBhKA0wTebHPl9yxf8xwKhHXMi%2FelSsl2UU5%2BmwwPw1%2FeQNWEnqSp1G6FWdVcSujjkIFytsLDrfOkTLrEqq%2BuQn8ahuRSdxP9JQsLleB0rhiwOLsAE2Kxlj0L6OXtfB9I9u698fL02fYPqlEat28noV%2BSeaRIeuCKoXKz%2BIskUl6NWMcDgorXJQgOzCcQFirFNdSseV6jNSt1p%2BE%2B5eU2Jm34111vpl2h%2BniNAYZ1cR3%2F4HA5LwfQ8u9wcUsCMUYQNECKMli%2BHGDxabrFKPPL6OMq%2BNsgBpGeWqaBJO9UrkYyxhY3oHYpi8eRZJxxKx7tsabYA9rzg9I8mKBVlL4FPP7TU6RIkTNEVWzuaMPs2ezKWg4JvBYNdgJngW7yDyiV4LF%2BjyJOdJV4LbgV9XAkYUL4Z9RA6BRELvGCnE%2FyJF7lKp93H9upWUAmSNfHZcuAMrnBcHhBqoVsgE74VkYrPu0MJnZq8YGOqUBfMvhRww0LLsLfWGocs1f64j6jG26g0j1vTriZr7cm8TdIztZzsAepy%2F8ZC0ClHkKDkdA%2Bu3hUqZW3iDk0OSXfd%2BjEBsOlOHY%2Bg%2B2RRPsa9ZB%2B3rcJPO0%2FjIQRuRNyxo7tMqEVvkcvxDWZBV97z8dLueRUkC3t507iA4UjONJyPv9ofP0pa5JBgVCgDfCXWnVT7JfYMQelu2MqUKtpEY7WLpWAF8y&X-Amz-Signature=526c1c31ff90b334c805ccf4e3acc5eccac5259589d28fb33a514d0a78bb2c82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

