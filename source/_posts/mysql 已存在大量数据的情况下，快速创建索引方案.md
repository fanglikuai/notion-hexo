---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667XEPYMW%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIGtn6Duf%2F%2BhySN9tue7wr1S4iAnfHLDcOjcUntVM6y36AiEAk7F3kyIwSIzswaO7SYjIDJl4J57Znf%2FdqeVHjkpXl1wqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL0upvA8ATrUqBOFHircAypEzIQSSSEXY2o08esADAjnneJoImaM7jM%2FlegjrY2B%2BZT%2BVZnY4gtDlBtIHS11kgYL42EGTvW76BQvAJf2PL3bGQMqJElPDTYxx6BPG3ZCWvjqrxtnmolzRCa2X2HG0BCZ%2Fp7PZ89BZO99%2BgDZHGQBhmJzlGf0%2B3bg8PS5uXUffxB5taoxPHoYpjlXesoK1USpSod5q9x3RTqEcIkkbaJLW1ca86m%2B8rm%2BLUg04nchMRpNKOT0D46UABlbhPv9tn72rZc2Jz9n2D7kQLwlRnrr1m2uEgcFZWwCgXC2yKQ2yANEkXnfR5Ap%2FG%2B3vchbNbbGxhl%2BjcMVv11XALHzUp%2FXgF0e83BlWlVdFsb3YMf3KqZnbNCVsQr%2FaTsa2HQKlNOX6guv9A%2FFCsgl0YzhBXKqDNxBR20obhaS62RV8gmXJXxqqGMBrZLN3Sn8hOg%2FBs74v1j2p%2Fn5VMOhZ3fbAqiZgGJPD6eO73bs24yebeFuhcrloKH%2FzeQ7pHdYCt5sNCRmZ%2BYhcgilMPDwismo%2FZ%2B57Wxlp6FcJ09CNwNm0plfYFQ%2FuZRoU2MQf8TuIdyLvXWzxppybV%2BpjQXsLgFVxy9mRpcEgSBU2a%2Bk%2BcHkiocbCPq5J69A%2FiAqWkWCMPjn4MYGOqUBB9gkrxnq1%2BHzZYQhzE9lq220%2FyS7LxmiyqyVsAkYDDph3aBZg%2BfXtsN8M4ML2KF2aGUZ3u12E0wfNs9paZv2JS7v5XHi%2F2Qj6NMg9RKfWfjHntzk2b2zLfJmzu4fwkB9wq%2F0A%2BF40%2FPIV3I9UJub8VsjIFNa3a5PRv1t8TbhtN9w9Iy8rIoPdWGY88ty%2FAWio7Ie5svGOLCocSUVTiZSzwSogpz2&X-Amz-Signature=19309e152e22e2aa28dafc82cb61110b8e37db4942ca0dc21539910ddeab30df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

