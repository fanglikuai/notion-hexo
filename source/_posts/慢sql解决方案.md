---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBZ7TWHQ%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAiQ%2FLy7uUU4wpI1EQewzI%2BdbAs8gwBX1wMVWQbiB2rqAiEAnIpSH5NjEZ3h9x4mziPSpsOBgU4N4f7gv76kpUBXH2UqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLGeS4ZqCI1FEqIkYircAycOWIjy895sIUykMQ8sd4VmyA2ysL5ab8bJLT6OpNPnSk4Dt%2FQiKNaNcu0XIaPb4pfhD4oLge6CrD%2Fr2ZI0IvWPyBnLgvB4z7i8fcTVkRO%2FKUqOdmzmAzsLWRE3qRozjEb2CHJdDqaRJtOGQqTzwJ07o2MWasoyJezbsd1moVbGCzg3%2F5IrttTnqHbEFVUqT%2FUmGo%2FCbHqkbcViAZB4Ka5iokVkkU0vVI0e6aDbCPpHvVFWNTkbSXUpDBG%2BLtpu7XPOk2uL%2BKQQxnrsSk6O62M8eRcY1zyy4UkxNepP3LtB0L9V5rR3Rm3G3Za%2Ft9VqfFMM7Zu96xW5Mh5h3DDMPvr4IWHRgm16B4YksUclwbfaOkKGaHAjETiraRfg58hrY%2F6SgwUJFQffOEpGBia3lgS8brRjAspls7jfzJqpkvWegrTXxbvBpOFQKIdNQB%2FeOyjS04BVVjJLJfevQHQwZKCeYCwo1MQpdNsklxc61dv9rnQslcqLcvEEY6Ax%2FvxBcw9NwyowwzJfw0UaDk7za3XLTPuggm7g4%2BMNbDjn%2FO33H2UXABZkN9AkhEroPx%2F1dxU%2BSHtvJW7iXAnIpLt9Ddxw3%2B%2B%2B0A%2Funq99J3Phf%2BeiYXSV8MubnrxrkcLFMM3%2Bi8cGOqUB1jX8s7RUlKKZrd3yo5WYbWvkci9sk%2FnwONijJw88lbnkFBm9cFYTONUpHTjwBQ5IhBjVIbeee2dmCxdrS0cr6vA3EbWCpFNI5FCaUBbLwCGutN9ENb6QneOKzf%2FMg3KzQdpHK3Rcf3ldhN05wsXtDTCnK0izUzVHC%2BIXc5ZhEefdtvM%2FitWatyqtShLPHeiq814Ok8BTjJnFPnSYHvEBorR4AMF2&X-Amz-Signature=5985f2a6b089701ea7e516e0267568ac125ea1c683ec95f0e07f4184b1cbcb30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

