---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IDTRRCA%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2VQsUvQtPRKst4B7xsrHeCGqvIihupKTM8cIV2mbioAiAPFtaCeooQyy0dB8c3zDj1xLs5fOsLQBWQ8%2B7t1vgC6ir%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMmwW%2FEezX4eNp%2F398KtwDa5%2F%2FoXJzMD%2BIcPuBFRIbj8bOJXaki31aSm0w%2F6K9mUMTdYKg52eqYJZsqN9C6MhqEEB828pW4M%2BgKoBuacFNncCupzVWNyAbX2xYMi%2FOiPjl3YwqajQt6nBi3uHJBiV1tKHjOLZ%2BCLPUKRZOziAUTDRj34kxeVtFgSdvwUAfCDRFRzYLn5JuFkjhRGW%2BRZ4X4EjzkC%2BkTJNxmQiH7cbLOaM6M9fdlUS%2BlF3EpQnHkyDlhOAsIzjVUBgcY7dZ0MzJgrzV4b%2BmM3DqIeAFekvJne8qxk%2FOhSGSRrWt7NQQfB4w7jY39DjCGBAaBbSLu5kwnq2VVuU57XaGDpyPa6hYUxKrGf76ai%2BGl72u1Qoh%2FOKJr3RSKEbyD4PDrTp%2B5s7rW2nKJPWaqBoC8%2FbUC8E97Pk4md0yOBh6J5rml45%2B0hVCGDHUfsDkAISj%2B%2F0LCYsmCLxj6DASpTwN6pcTjK3NghlayG5WLQMy0ExVxZauKRoiWwaR8p%2BRXgxvTvgVderIBQSEVn0IsEcA00mc3gG2x8fSPEcS2Drs9cuSkAx%2FUjAVmBV5eqDD2VOMMbZinrDx3joG%2FZdQ9lld4Vcv7bP6bK0%2FrwCwD1AZQFiULfrsPsJZGz3dc2kjN9HmwPswh8mYyQY6pgEeu3TQkTF8NWX1nnWllqpI4i26cLdGu3eEKZW8gcKmaSP9nnDUeTkE32vjasGOZbb7w6hagxlBo%2Fj74BhJ6ka6XJlPRxCjS2%2BtohbtBwmUVgYczl4KS53oQGXYjnjho0BVprtYbmKX9y2FJzMwiQF%2F%2BJUK0CBmmPMah7bcqchWzuS1k9Y%2FIBSwlUymYYk40RLevqOPS7D2%2FILDuFvorwJO5%2B%2B5IMH5&X-Amz-Signature=9a4b37cc8eacf1532a69bb8fba691ee08c0211669d6f7eb4aa44c747254d8920&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

