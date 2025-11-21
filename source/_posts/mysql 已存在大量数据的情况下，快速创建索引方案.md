---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YVS2WFD%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIFqmPENWcT3fkxQ8Qt9oK3Wqh0Ky%2BoMMksSUgvK7Mlu1AiEAygJOqP%2BFVH1bAoLmWN%2FHFqFbcreANEzw%2F2pxBpVy0Hwq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDCWOUhLUGCku9YGT5CrcAwUD485lnNYBhzAgDbfbI3TMaBwfUNnVwx%2FAh7BR5J%2FnM3W0tJfoVDegFke1yGZ6A9RwovWIi5HoR%2BXJr2Dn0glB3bvpV7pxtFzPmhF7hJekqIUpsW3efz%2FseRRNpqzkpruZDwL3MpJimcWTFklJVIghWGRqV1VnLliIsBtrIWxPY2M4C271KbFTZ%2FQfSJhi3tU1F26ndU1vnvldn%2FWQUx3zJByauU6BHwTzKzbhBcvrJZ6ak4mhHDAoFIiQ%2FSm7HyIhuJNmDLb3NsH57i%2F3zXVt4lcvE2yZVz8cwj5ggmKDJ6RF6eMcXTJGMk41d0YC6ytaFHPqfOERZPhqChRlFk81JpbbGP0MpaU2RVnORGPFGDyorBgKTZx%2FNmKwbkqBFlB7ZOYPqeswdGfho7VFHkuyA%2BXAjtAe%2FGT6T5oXmm1%2BzXFLq8Rj3Ifi2xAPICjbBQ8woywOrFxR0HT7xwVam6%2BNI7rplMonROVFa32WnlJRGgQtLVzPcCfNrkhQwwJOs6g9ibCMeGr3gZTYfwUVSOIuadrOy6OBuDX7ib4Ye%2FaH%2BnQxgaYbqP6v2%2FdoTkJHzs4XxbkEcVuONmurDrLmiNTxg5gIMiXcN0E5%2FtyL2a8RXMAIuycSyOV9bN0aMITG%2FsgGOqUBMsSCw7TwiXtVBeK%2BAQZKFbrlnGM6sttXO%2Btz8ZuOSMZRhkicWEpC8oZ1yCEs%2FyqHahNYHh36SBIlWSX5Z29Hzo8K8dM0WLBX%2Fx355GCjIw8eyFPUcwKpu8BTf8b6KLvZTEzZ1jmOfyu3C6PWYCSZ9Y4kEpWDuaX0sEWD81fRASRYw71EpstW9yK6W%2BgFijGbZj9KCedR4W2D87dvTzgwlj8KxKO0&X-Amz-Signature=5c8ec3a7aeb712b6516d219e49623b64f8a08e26528f5b68b0cb318a71cada58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

