---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ET5XT3K%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFhNC%2FM3MtptwsNZvbcFBEl%2Bjyp2h%2FVxr%2BdmOrhE%2F%2FsmAiEAlY9e6u%2BMgADlyMGE4sYgWfooKyWb6au8OJR5xwvOmWoqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOkTGCkEAZ1bX0mU0ircA2SHsH9h0XAPY00r1o4Ftl2QbZ5UGxc7U%2BQt3nRAoX2MqfSloZzelEmSlenOKxuJqv2djrdfvDr14vdfWz0cL%2BV8xcRrrFtPkBmrGUBH%2B8bk87%2BHk7SIU%2BGF0ciOHpMW6GggILchmKO0N2BLAlVRUpzZcvAyxx%2Fo9iaxcrNRitXmdsCL9n%2FjkZ0XC2fWRscwroYD9eBaHJaVrD91Tj5W8UhcB3WTXnu%2B%2FoXWorRf9toLx%2FGhcjE63x25L%2BUK9Ga1ulPnTGgsL9LDVoGm%2FmD1cyrO%2F4r6Ao2Q8l%2FiH25ER6w7NtyaTa6v58bymopRzFQjjl9%2BlJcYO1s5wcGQiaa%2BpB5hO%2B2kr%2FKla3YxYauBwcEDsVQMLde%2BFnER3OeBfJvRxzYeYMMetr9STLNWrTHX97mVx7PctkA7xRatxx3UV7Z6KYPtuXccX5Q%2Fl%2Fx%2Bd5mhLh00wNOAnueEjnNAZYvCL4qZp%2BjA7ahh8DlHfrJSvemSyl4pDAN1SMScBh6%2B4sw2BaBfkCtInrDoetZ2YZaDCZCmwtEa1xibxETYl3KQzxI02ZmaEWtR90M40E3mgWO4z%2BvuQx0x%2BxzklZXe7fOrwTppPsSF8XWvCIa5%2FReu7C2zgMz7iVh%2FDHobQ6U2MK%2BOjccGOqUBAwwBV1mGRNCppw0nKn0xchPHqYsjfR6VejOYiWo%2FZ7oe%2B4WuGMJAfPtmE0zaqnj9tRxfY1qtb0mkcsbtC2FGatUdT%2FKIBedlszsG7XCPkto7sr4fz4kckNhqfT7d%2BH998d0YGRR4Is5MuQHuAFMCxTWSsHZbZYnhBnkvLX5XvTgoGkSCmC5gC4hND2ujttrMo5p%2Fp93bcUqoPPknWRfVtIhAMXBX&X-Amz-Signature=56b677f6e78191ee94d3ac2b0ebe4dae2b5fbeaf66705b04daade60ea1490a25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

