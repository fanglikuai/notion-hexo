---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TZLKEHNP%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIEqeLDzorVmaXEyIWhDbWPcnRUDl9zlAXQ5hsky9rqi6AiAUPvfrjrSOAj%2FJ7KctaOkFIw0Fo0gaH6oUM2RKH%2Fa7XSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXVkGRrT5R%2Bygr4NUKtwDO%2BoRoqv7TcV8R2Y8f4LZI6yOMdjpVkPJ4Lp3atREszBc01ee4wX9%2B5oKC1VGqVSdmufN0b2TaGYCveWFizjkztTsVftP%2BYbLKWxfnypAtdAvbcOl5934e5x%2FzxshSaK03NxyCVRjz4EsY8S7Ve7uVUKCU3yU7X26RthFkkMY4FWJRQ7ST%2FEaOSpYBUG6S1RhxezB9Ro2R64PNdBkFlqy0ZrAn6t1vZ01jWtPKpekh6Koxeq8cN8jOW%2B8gdXjsZZJhZzoAUnvqU1uSVo5erW1Ts7aHp9X8wffHcmLDUm5yb2xZEImbt5OMh1%2FSaNnR3rk5M8MMhHZrVzgC%2Fl6DrbrcTacGOToJ1WYzmW12lVCz2wJiWYSRu99Zb%2BkJQNxIBV3ug%2BsU8b0NIPWdHTdGK%2Bxk2MIOEnCm%2Bj3ytedk69zJV0lmkZ2aPGv4iejrQx8cRxvXiwVUtanOpwXyn%2B6erRhk82wSabIKmF2nsRpjxiFpGC2d5He3Bk9aH0c%2Fya6vuC%2BSDctwjZKc26zCbaFvUnxgmjXmrvl%2BdVolR5kfDx2aYC%2Fdl7DqgWRyPhIJFm3DGPgCi4XWDd0uPBGefTz90sc4Ls65BJC2BXGBbbLa0QP3MiKL08jI3c6UDkkNOsw7d%2B9yAY6pgHtG%2BKwzthI3pd8i9oBm3mCIosyUqbhXc1DXm0cpbZhC18B3AJ73AF8mqTVk6HjsGpHeAug4nb5W2VzbiIAvvT7vysMYPkExBvByhsSmJDltKT3X5TLkoHVMQUF8tOK8ydtdU%2FSQvRcgo2zY0CC93KY4SBan%2Fk4CHx6fxBNJcLQML9FbhKT581iM2u7vxFklb84cCNncD95Li%2BBXJaC7UulFi4ixuL8&X-Amz-Signature=8935eabb2b43f762c885d933064e7b5f0ffdab60f044bbd25a402740fdd4e979&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

