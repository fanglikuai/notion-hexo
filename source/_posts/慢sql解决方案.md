---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGYHCXCO%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDT5FPE22QYV%2B4fDZ%2FNNyh8xVp1GcwkDrjFwKXFmiIq4AIgFtswhwL%2Bd3vtuL%2Bd%2Fm5VuhLgifZI7SoULVaZV8qSsdQqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDNdsmnvrQMoSQzMGircA5JaXTUOPSl%2FRfjwPZFk5IFEhFZe6zm2e9%2Ba1e8MqPAw%2FQMH17F8tmSJAlukjhSgm4lLLw6cDm070zGIHifMHVBBcP%2BZUasTnq8chdoxuM0pdDYmNh43VKsq%2B%2FntFplNp3RcaSLJXLZNtOANchqanXIZAfrsPASsdDidY%2Fifv5crx20qkIAW6wUr0K%2FUZezcnzlYNbySMISz1X2QaoZa23%2Ba6PNAlpbDHZ8V%2F9iPv8A8drD5C2wU6YzBGf1K0lMsomCaAMcu0nO4bebuUl2%2F4%2FlY4Stkj8Ik1Aw0rSgYedI9Dhlh%2F4Zo2pFJ%2B2NfsadSbdAI1L1WnqE1YH%2BkzLlpo6KGwSdKY4DMuUV6zutcWcM7DRDXP91bROO7JWYmbz2OL5izvdfQI0riXrJ5%2B4okR9ZKCFTpoQPISw8ANrem4j5JB1wAvnipQwT3fbi6wzhVK8e17XWNzVUfJBdup%2Bgu4M%2Bho3uuJ4UsuacSuqShyzBgOz3WUEx%2FaYDH6PGExSfWb6iGCDsQGQVR2LF6JZ4R%2FI1x1meBoGb5b%2B6JmHAwyxqFJJZPvQlg9VKeX7m4sI3AGHel7bwLXrxuobZxxXLcSaDyK7H0CoIlCr49w7dA4%2F9h83u%2Ffu8pRcklCHcYMNeAw8gGOqUB4wH9L5bHymILTWiAiWBnbPJxBAVFjldzW%2FsGbLkeL1BvQWce%2Frra4PcbHuBIzGZ2d2OBjwdG4h%2F2YmEwxs%2F%2F6n7CXd7X5AtSsX75bFrDfKbM6rlJvXed5BaeM2bWC7dz4B2JqJpn%2B7sRD8oUTVIsWUgnW4NpvZQ8nCRzuZEMaY6odUbfZuB2d5nXrVwGLIvOztODo%2B7sJud%2BTFoOUlC3A3c0Kp3X&X-Amz-Signature=afbf50b8d7b660d7f4b5aeb401a7e5d3f474afd90765acfb22dabf5bc26a2b8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

