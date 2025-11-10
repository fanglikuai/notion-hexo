---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWNSE2LE%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T010057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIQDK9myl36veaAW2X7Buw4fmZ65%2Fa2EeEpd30VnF9KZ5BgIgMTxRgG86SLeil2VFWU80fp502P%2F50HCU0Mzx9bS9KtgqiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK6kB%2FdmJebxub8GCrcA%2FldPO9AFIMwY9k9OE7JJVnZ7K%2BBrSjsxkPFY%2FUmmP6KOl8%2FEUQ6%2F2%2FS5oBVqv4cGiiLjGomXE0z0yHXK3yAijS%2F4020B%2FopNlMhCrN7cN%2BH5W3XxI1QaL0kgcSSysZgzGw13dKzJByok8dKDw1LTlCmT65NIW1BaBKqZt3nMV90YuDGJwcp7tQ1GVSS8O74d%2F%2FSW28oBH2usX%2FlT539H6Ve87t2jkYwLtng9ieGqdXg7ezMzPZmNdH1UA1GqUiGx2YXIkpIkEzJLPSdcrYL5V8avE4q8E0JdYAaduTk%2BpbK1jlaUjTp1uM%2F0IfJ7u2sdeGRjR4Ad40Kbix8IxYF9Hr4k7qlLqtF1Lg6ovB985S%2B6eBoogvB34561%2BBmqVXrta3lS3cgunyQfv10yObRo%2B9n%2B%2FbXeauNai8QsynjfbQVNLKivwVJx3Lf7urhljk110K2%2BoaBw5xrAfzlfLbZiGrI5DbZYPxFpKFspd4KWbG%2BPn3WxM7p3qZrJcAHRip%2BudZE5BEspkbKBimJe%2BEIA4BUR5xQ%2FAbAwPEgZUzVeWlqR46M71%2FrVVZvXcBq86ItmpBe6hew%2B4WCpXwrwVvL2H9n%2BV%2B9ka7Yyrk4NNowKWatbiVbdZqXnaSjsZaUMLnvxMgGOqUBzY81LJhp%2Bo0XDhCgXJEolfIRjv6yu%2BpNuAsc5Nb88iStNRfjuWZHnVvBgJXxadbHgo9U8C5qg42Wm8AzM6geUY5RF7l3jNb5HJIfAFaQ8KUoDr8OSqfStsQhPasjhWXa2f2Z%2FD06sYvD8bieYjZaAwsIoJIs%2B6smizkOEcER8fBf26gL%2Bs0%2FCRWGFbHYaNsiJeC4ip4WRUPNcpMXPc%2BpkT3r8epH&X-Amz-Signature=41b581c98d77369055a8a0eff1f630de17b96cd4c89f810cee6114cd2e52af23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

