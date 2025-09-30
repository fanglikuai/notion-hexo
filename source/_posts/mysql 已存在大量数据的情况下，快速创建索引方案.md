---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622JHCDKA%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T020517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIH61Y%2BNPj8zuXptWap9WVHLlvQCwqIZmP6Ab0tSFcbLGAiEAp%2BW1ge55KBQOxvDtJUZU9P%2BrS6mt515QsN7%2FAiw6NRcqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDPIcbxDhnus6ZYafCrcA60d7UjZ5WgRGElV1tobTkFD1B8LH38vSTDwnv6h6dsB5Cy%2FFC4umySoSJw4UJZGFQO9xlAw5s8fdToBabZUyyH6jSxyoqxadQBkwxKU4XZL%2BzAe2eZz03R3VuxibX%2FpRQaDJBRFRHjln7pNlfC5IkqCnb5Lv9FWAyDhTbZozXL6ASBOyAVuZVsqQFSr5DrDRqydk9KBe%2FuoIQFHpRSXjxdTNg1WM3BAPvynjdQpUNoc2xj0LHnvTLCk1Hdc0L3Mf54EqdFvqn%2BCyjgqWWeYPVhkuwiOrDNQSnTT%2Fwgiqmhgy5jicELlINVRGBI%2FymreXL1eHT0JS1mwHgfiOuNwviShozgKwAdEXwdmr1VLbY1HqwEUKGqu0v32j9FwTE54I%2FNaWmy7REV7cwfjUwy1tTooDWYmoHEwAWG26qoeR6b3I%2BZk107%2FYOgr6%2FcYq22zeOPI1Thab8cpq01GUDel9nQ7d2n17xlDC6qK91M4%2B4o2dSWD1%2F8dHlPllBz%2F5n374BH4p3%2B3SGcCAohnhaB8wW%2FC%2FDaHvTLCfsT%2BQg%2FCnWpK4LeGOVE6mCnE80Yuq%2B5gJtZB%2BpxKTDT1vXM49kWLtK931xWxDr7nuvitdYJ6sb41buZQSjRvua8ysrVNMOrn7MYGOqUBPV2LMu6qzl9LLvx6VVLtzwtY3lfayPm%2FJmJ%2FfrHJOv%2FvPKxJdu7JqHlp5XQlCse5IdJjq5NLDXwtU4sSosCKqB7l8OGMQGI9Ah9JjBUvny1YJGQb%2Fq6NbddXtYf69AmN4qJF3LHmZcU7PkgfV1%2Bbf5ZHQGAB%2BLdfpo7i8jXtdycfthuuxXtWDIVN%2Bal%2F8Pl2GewdG58n5mlZlsgDbZrRp4cp1hA2&X-Amz-Signature=cf2ea7baa883305815592cf62014d4c07ba56b157090baa36ba06678c2ec346c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

