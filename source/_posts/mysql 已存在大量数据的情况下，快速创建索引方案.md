---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSWQ5O56%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsmHmYDsWfszG9FaaA1IC1Nt0dXZ%2FhnPWqSoMNhXDaTwIhAI6ednnCFXWwQeuXW10lqWslg%2BtpzvMlnZQ5aWc3xxFJKv8DCGEQABoMNjM3NDIzMTgzODA1IgzPJd3gc4WvGcYA43Eq3AMeJypH7yWLinYDIOhhOgHERHwxnzTY%2F2xRdyvMaS0Zmrg52g6b4SunQDCHLdnm7N6tRcaxaWQkcA5aQdTPwM8U91OzvIH13MEJNjb%2F3ubDTE7yyztyWwSUkBm3%2BvibBJV%2BlRPS%2Bh5thXScuP%2BzsH0esOX3VcvSS6utHRTmDVjkva8WuTHjbCYGUEKOa8pBdd09xGUpmUp6i1cRJMPXqTl3%2FY4qXzikyapwCQKQ5Of9pAQnSU5tZi5zcWVEyIciUxr0kX4mFUdhAaYa%2Bh52f7fZtCMAK1yik1HCqykw5MgpiN8kI02CNnDeU2VVbtcpQYy6sUl8%2BLBPb5NqJqKWFL6955%2Bd6SxaltxgYeeEwWRuxagi%2FY%2BC0MSRBGayLo%2BOeoUXJPZ0hKU8O8SZ7B2vte24roY9vgTkKnApK%2BkCvJsj4QhEEF729qti6NZbpS5l6X1yCNnR0KRZdnMwI5VhfuGoYsqAmWO2ZWVDPsysn8InrY4H6jhsqjMY78EDoLmBUrkGOc4XHWBolN4UNk2mTytT4xgFQf%2Bj2JT4qrGAD%2BE2J5tbxUgDj0ohnFp74GprNOPgwjaBbfDLF25yRndXmMMRGMk70BUpOQxzmqltjV99BZhJiT78mQLvnvcycTC4kIXHBjqkASPa0URqAJv0Xgy%2FjOOzBA4VFFh%2BIbf3tpplv4zAjsH82Kl0%2FsBElm1caN87IEtb60FLxC5F%2FKrOCvygUlL%2FoCsH9DLJF2xlRs%2BOeF26OuDoGgyXDdoFfxJGbgNhUSnvqx44lKNH56ER82YNGhQ5UdRIwe1O%2BIt438FnIq34oeFdmBKmje6%2BTHk%2Fn9JxXm%2FWuul1X%2BH4b29ByHcy%2BbxeKEUGogsA&X-Amz-Signature=549472ffe65d7ea8f0dbe643383d11f8fccd0133f5356f04631c6b8cdd72f1ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

