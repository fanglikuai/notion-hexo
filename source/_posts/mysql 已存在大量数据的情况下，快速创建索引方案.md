---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ET5XT3K%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFhNC%2FM3MtptwsNZvbcFBEl%2Bjyp2h%2FVxr%2BdmOrhE%2F%2FsmAiEAlY9e6u%2BMgADlyMGE4sYgWfooKyWb6au8OJR5xwvOmWoqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOkTGCkEAZ1bX0mU0ircA2SHsH9h0XAPY00r1o4Ftl2QbZ5UGxc7U%2BQt3nRAoX2MqfSloZzelEmSlenOKxuJqv2djrdfvDr14vdfWz0cL%2BV8xcRrrFtPkBmrGUBH%2B8bk87%2BHk7SIU%2BGF0ciOHpMW6GggILchmKO0N2BLAlVRUpzZcvAyxx%2Fo9iaxcrNRitXmdsCL9n%2FjkZ0XC2fWRscwroYD9eBaHJaVrD91Tj5W8UhcB3WTXnu%2B%2FoXWorRf9toLx%2FGhcjE63x25L%2BUK9Ga1ulPnTGgsL9LDVoGm%2FmD1cyrO%2F4r6Ao2Q8l%2FiH25ER6w7NtyaTa6v58bymopRzFQjjl9%2BlJcYO1s5wcGQiaa%2BpB5hO%2B2kr%2FKla3YxYauBwcEDsVQMLde%2BFnER3OeBfJvRxzYeYMMetr9STLNWrTHX97mVx7PctkA7xRatxx3UV7Z6KYPtuXccX5Q%2Fl%2Fx%2Bd5mhLh00wNOAnueEjnNAZYvCL4qZp%2BjA7ahh8DlHfrJSvemSyl4pDAN1SMScBh6%2B4sw2BaBfkCtInrDoetZ2YZaDCZCmwtEa1xibxETYl3KQzxI02ZmaEWtR90M40E3mgWO4z%2BvuQx0x%2BxzklZXe7fOrwTppPsSF8XWvCIa5%2FReu7C2zgMz7iVh%2FDHobQ6U2MK%2BOjccGOqUBAwwBV1mGRNCppw0nKn0xchPHqYsjfR6VejOYiWo%2FZ7oe%2B4WuGMJAfPtmE0zaqnj9tRxfY1qtb0mkcsbtC2FGatUdT%2FKIBedlszsG7XCPkto7sr4fz4kckNhqfT7d%2BH998d0YGRR4Is5MuQHuAFMCxTWSsHZbZYnhBnkvLX5XvTgoGkSCmC5gC4hND2ujttrMo5p%2Fp93bcUqoPPknWRfVtIhAMXBX&X-Amz-Signature=ff1b283ffde8287162bdbefe7ab942bfa78a788fac95431f039f09c31c6a0537&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

