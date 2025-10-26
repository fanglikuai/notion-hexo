---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI4HY4B3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICWuks8%2B63%2Bs4ZMMd93qGMdQAVBlA5Az531mGeg3wUi7AiEAk96vETE7%2BZAsqouef148Ux6d3aMw4pyI%2Ff0Zxgt295UqiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHR%2BX1X%2BMEDdfI4pICrcA4Au3r1Si%2FSUhETG8Lq8NiNTLp612jXuGsL47Ec%2F21PMb%2F%2BQZ5chE0JzVK0j2n0%2FUetMbCBBPXpzwj77OZ5P2q7m2VDjxNWcO7Vj9jnmAxOy3U7iAJtidqcbhjpQXJx%2BW81001Nl1QD2Sky76lWkFBikLkqrAc7KJ6E44JGnuP8TgqyTVD6GZXDPn0mQiU6PPoFuAJvZK21fslp0JsrUkQFH7t5RZZyabA9vZUiu3uLYD1YTLKIQ2xKlDMoIobbSFYnd2K1G%2BBF7yH%2FwHYLrM8SpzmrmQs6oWDjFCyLS640SpVC9Tu12MZIwwRFha7jTs4M1LJ%2BdBbDMqU%2FdnHepdnnW%2BA4qWxT1i8PHVXeqv39Fv7OZEqHUyrmmSaYkICmuoXCVeUGKJ59wko7Lh5u8QDSugKESFqMxtNq74iLktThs7g5mhfoFJCzA149TWH8r2iX6TrCe0iMzqzWebSnTjDL56eSBtlUimctw3jGoeCwA16MOn%2FqxaUgdyv8YPU2X66Ffgf01rJ3vYRYQabkcM0danHF4XGiEKSBteSByxgI3PWa6DCARtqcXMH5TgltxMEqL7r%2FU0E8Dn%2FkOive3ifBiQKpz3VopgHdKFn3ntqc7lRsdN32Mju94n2%2FwMJrO%2BccGOqUB2jFOWZOIStCaOhOs8BsQR%2FYVjW1chLtjLu8ttHmqiGi7HX83eWq9GuhrWCFX2bxlxc315KnP0U0FXzlOiCKzKyC%2F3ukyVNqVcOL6qibR90zsVRwtadqkep06OxyPm459uapDLos8vGFwcitGns9UZmBUuFvAjV7R9xxYqc81VU2zBa9onrAzxIsi5US7XlATZtk5jEcYO1AJqLX8StFjoop%2B%2BPe%2F&X-Amz-Signature=f63e94a8f5646dec74c9cef6c7b7705d2761ddce6622a880c40c1cdc10255fa3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

