---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNWZR2IV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T100649Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAlyV8WVAAgTreZciQnoRzZPBT%2Fjz5fsqd4ufmt2Yjn7AiEAs1LavMa8IowoxrXNbk914z5roVR3roHO4uA55kHfa7kqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETky22Q6Ro45I3%2B0SrcA57AR9Jw9C2rKjeq52nMlchSSkWezIRERT8xnSV9moTwcuPgQ2oOUXTJdwjCb%2FOV0Mwc5wp0z21aHpRREGhpTPBMWcjbbhfpVDm3Mglel0ZIAgP5Cl57mmywVjz0axqkf35cvi%2BjgUxE873uULd9ebzgLtRleaW2p2cJ%2FoP5WCvCPTBz0tbzZ1GrPkEBFdNA0CJc01yQErev2s44dbid0v%2B4%2B2EZ4HK%2F5LcA4RYFfFJml3k9xExXC%2FOo%2FVCmSt4pkUwCRmrA%2Fr07j6OOnnd8QWiVX7O0qVHM4LjhRyKC7GHY57rxnyYgR8Ke2mOTHkGmeGqX6eBx39R5Anzyl7aMMql48mQKO2QKkUuoLgQi50o0yTK9FQfKa0ssh7OVed5s5L%2F03Tim6sFV9FWvQXEJY34UgcoLLvBitNys0NeJTYIjLxO3eMjAzQzUeB3dJRcn72YF4%2Bhi5X3eq3OvC5tuw%2Ba6%2B2rXroCtQ09fCF2WL8QlQS8cHsfJQHm9OkjDBhVU8Bo4eUAJvO%2BNWaBoKXR5w7d%2Be0wS3JpzBByR5dhEd%2F7bKNh2XiuTczBrOHzTlCjgWno8UPZZBf9aS97CN%2Fkbtcge52vxXRt%2FZS5vL2KWIfwCvNQB14%2B%2FF6zct7ciMPa218cGOqUBBUNT3%2Bo57yrEN%2Fzi7TqKPxPnMA8UttSzzYy6ao3i%2BAYAA2Hqpd0sruQc3a8iZoZGjM31WkhGbi8cfa82d3WtPTlACTv6wWiW3vP4lSM9KEeIxL6ZjtlGolxBLzji70jeMylnL7bniE6QNlibj8949bbyuyX%2FbQC50IuB7Z8xvydeuhFyEGIIWimszKPdL26GDR53PERdsFnoluKct8Z5YuCxjdH0&X-Amz-Signature=b7ee65d6a9c2e8a7dfc935d6ae030e9b2b345185063c5d6c5681d7af134164c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

