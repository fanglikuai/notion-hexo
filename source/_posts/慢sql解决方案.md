---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667XEPYMW%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIGtn6Duf%2F%2BhySN9tue7wr1S4iAnfHLDcOjcUntVM6y36AiEAk7F3kyIwSIzswaO7SYjIDJl4J57Znf%2FdqeVHjkpXl1wqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL0upvA8ATrUqBOFHircAypEzIQSSSEXY2o08esADAjnneJoImaM7jM%2FlegjrY2B%2BZT%2BVZnY4gtDlBtIHS11kgYL42EGTvW76BQvAJf2PL3bGQMqJElPDTYxx6BPG3ZCWvjqrxtnmolzRCa2X2HG0BCZ%2Fp7PZ89BZO99%2BgDZHGQBhmJzlGf0%2B3bg8PS5uXUffxB5taoxPHoYpjlXesoK1USpSod5q9x3RTqEcIkkbaJLW1ca86m%2B8rm%2BLUg04nchMRpNKOT0D46UABlbhPv9tn72rZc2Jz9n2D7kQLwlRnrr1m2uEgcFZWwCgXC2yKQ2yANEkXnfR5Ap%2FG%2B3vchbNbbGxhl%2BjcMVv11XALHzUp%2FXgF0e83BlWlVdFsb3YMf3KqZnbNCVsQr%2FaTsa2HQKlNOX6guv9A%2FFCsgl0YzhBXKqDNxBR20obhaS62RV8gmXJXxqqGMBrZLN3Sn8hOg%2FBs74v1j2p%2Fn5VMOhZ3fbAqiZgGJPD6eO73bs24yebeFuhcrloKH%2FzeQ7pHdYCt5sNCRmZ%2BYhcgilMPDwismo%2FZ%2B57Wxlp6FcJ09CNwNm0plfYFQ%2FuZRoU2MQf8TuIdyLvXWzxppybV%2BpjQXsLgFVxy9mRpcEgSBU2a%2Bk%2BcHkiocbCPq5J69A%2FiAqWkWCMPjn4MYGOqUBB9gkrxnq1%2BHzZYQhzE9lq220%2FyS7LxmiyqyVsAkYDDph3aBZg%2BfXtsN8M4ML2KF2aGUZ3u12E0wfNs9paZv2JS7v5XHi%2F2Qj6NMg9RKfWfjHntzk2b2zLfJmzu4fwkB9wq%2F0A%2BF40%2FPIV3I9UJub8VsjIFNa3a5PRv1t8TbhtN9w9Iy8rIoPdWGY88ty%2FAWio7Ie5svGOLCocSUVTiZSzwSogpz2&X-Amz-Signature=8ccd9d442d43a3ce3fdaf95478314151e2a93928262ebf6a724c535cde4cb6fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

