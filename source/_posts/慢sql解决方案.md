---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622JHCDKA%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T020517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIH61Y%2BNPj8zuXptWap9WVHLlvQCwqIZmP6Ab0tSFcbLGAiEAp%2BW1ge55KBQOxvDtJUZU9P%2BrS6mt515QsN7%2FAiw6NRcqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDPIcbxDhnus6ZYafCrcA60d7UjZ5WgRGElV1tobTkFD1B8LH38vSTDwnv6h6dsB5Cy%2FFC4umySoSJw4UJZGFQO9xlAw5s8fdToBabZUyyH6jSxyoqxadQBkwxKU4XZL%2BzAe2eZz03R3VuxibX%2FpRQaDJBRFRHjln7pNlfC5IkqCnb5Lv9FWAyDhTbZozXL6ASBOyAVuZVsqQFSr5DrDRqydk9KBe%2FuoIQFHpRSXjxdTNg1WM3BAPvynjdQpUNoc2xj0LHnvTLCk1Hdc0L3Mf54EqdFvqn%2BCyjgqWWeYPVhkuwiOrDNQSnTT%2Fwgiqmhgy5jicELlINVRGBI%2FymreXL1eHT0JS1mwHgfiOuNwviShozgKwAdEXwdmr1VLbY1HqwEUKGqu0v32j9FwTE54I%2FNaWmy7REV7cwfjUwy1tTooDWYmoHEwAWG26qoeR6b3I%2BZk107%2FYOgr6%2FcYq22zeOPI1Thab8cpq01GUDel9nQ7d2n17xlDC6qK91M4%2B4o2dSWD1%2F8dHlPllBz%2F5n374BH4p3%2B3SGcCAohnhaB8wW%2FC%2FDaHvTLCfsT%2BQg%2FCnWpK4LeGOVE6mCnE80Yuq%2B5gJtZB%2BpxKTDT1vXM49kWLtK931xWxDr7nuvitdYJ6sb41buZQSjRvua8ysrVNMOrn7MYGOqUBPV2LMu6qzl9LLvx6VVLtzwtY3lfayPm%2FJmJ%2FfrHJOv%2FvPKxJdu7JqHlp5XQlCse5IdJjq5NLDXwtU4sSosCKqB7l8OGMQGI9Ah9JjBUvny1YJGQb%2Fq6NbddXtYf69AmN4qJF3LHmZcU7PkgfV1%2Bbf5ZHQGAB%2BLdfpo7i8jXtdycfthuuxXtWDIVN%2Bal%2F8Pl2GewdG58n5mlZlsgDbZrRp4cp1hA2&X-Amz-Signature=f14715b76b050d1a13ce844f02f4d2db411cb25345d463122a9c632d7fc5fcd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

