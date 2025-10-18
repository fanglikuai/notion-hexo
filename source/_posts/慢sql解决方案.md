---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677TP7FRA%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCeTAJJVaWv9h82G4yajd8YvJqDLvHwI0XFgt7b5xMViQIgLE03DC1kIxOzZUtMdXx8TPZXEf4ucEUpR%2F4qb9%2BHivcqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAasM4N1vbB1oBAObSrcA4hrL56pcxh1QrtESWycjDAE478yEwD8MYWUIFCyavjvB3825gH5Z4H%2FKRiMiKXvyRU3nhb4BmtbGR77ujzWV5mrS5iVkQYgGKdNGLz3jrCSU80WGSm0d2yGjvX4Bf5rFb%2F%2FFl6F3MumuuPBB7kY9sGla9YyGd%2BJshvvEKGJakfiThChehyzAwfDL5STNoFHtHq0odqYF1HWPLp%2B4n%2BZM5mbLQWxJPls2Wn4%2FWPEE5S9g4S7Xo28hxG0a5dPe7UYfAM%2F3dPJ%2FCt9Vl%2B7WByWMs9fHJiVS7zS8NK0WDfONVNJLZlntTHCFyiHGOlT8gFGZZPeodaHzC1LAv45txolKh4j1NtugwnDPYP3E2OqMO%2F20t8pM8ED5gtTveJMJExHquhL8TPLVzXRcUoFmEifnEouBEHoJK6e40Y7Qu7kGboqxr%2BVJ8YewHcRRtMD0CHIjQOtED85mw7ordckHyv%2FjNTZWucZQ%2FhqFhCNCMHMpe%2FSyPr9YwsHAZQWZDGv6HIDLd4dyjBa3TuAYmdb1egGWwWTwKsKOuFVfhniXrDw9X2FW48hN9f6lUP4v6hWLOcQPUFKDnFSEkI5rd%2FbvN7sCXVsUFcYFc%2FAwSyfTt4E5ByM%2FF6Y5miazc74YaE%2FMOjkzMcGOqUBCpX2E9113GTMlaATD5QflAXLU6a8Guy5d6mH4B8RjiKvl9kizc%2BWU2bVBxvUGZkEIKlGVowoMdRtToI7UERgOr3K4lRmEk%2FCwyXdtjBrfbF1oIhI0NNZXK7Z9LhmSztd%2BqIdNLzXLjIN0r2ZCnx0ic7Wi4EtMsx%2Bj0Ii7QRsgz%2Byl3qirpm9Hhht6qY4Sos487fAeu66hLyuTE%2FvkqYiVpcXV8Rq&X-Amz-Signature=5b99e021c5f40437a2a756e8bfa5f374467531a0b7f61f9d5d0781c65fd04a9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

