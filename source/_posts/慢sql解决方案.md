---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRZJBB42%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCz0%2FDdC29sNZ2dp4zn%2Fz%2FUKtQU91xNvE1IAB32xwOqgQIgHs7XjnGqqzTvgurUd2K7RlWuVWBRPKhbKTGjpOitXskqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHZWCPQevMfmX8CEVyrcA2wSHbCVcy5BhK8QVY0Wj921W6zM3Bk041KRRkL4kU8aMtXLtXzpjRHu1VufLRRjT3nQzi2G1PuBQO3IPngOW9yRDpVLS1hvouBgiMNxRo9L43%2BeObnll8S%2F4DVxcYjB%2BXyMRNG0ey6u9WMe2oKAqrlBKmgIPyKuVBxfQLHu04JmaP9Hfr%2FEhs2QZnGRlsp3D9QDraGTQixMlYlK7nn1uqcR8N6uWHXGs2i4krvt1d3SWlQSIjWW1Z7Oixw6w%2BEaqsgglGyEXsE9zgj1PkzEchoSakTxRZn%2FUBqwCpqMeOUf%2F0QmPqQUn8YvdBIya%2Bb3K0vMGaB%2BQJ3jDRL5i%2BZJIJrcFUgCO7KKoCeCNIqvfpxv%2BtQ5SWJdOUAfoHO1Bz%2BJC8AuXR40fh1xbOQ49yXzMV9%2BeB19toiELF7DmF8NVvl7lBJDyYGF8BOdpEjnXMKSWAve68B4Vani9eAG9QAA9JU%2Bmfs73WNPbqPReME74PH1WS%2F8D5yOWAdPj2x29wOO4yOzv5wr42mk0IP1gKR0amCCHB5OLP4lct2bIoI57urLqT%2B%2F9t6IMO6fSeGuKtLQX2EnWdLdriS4dCC99ckZVBgIxl71POt26fv%2BQof5ULkjMApYmFcWQBZ0rSW3MILYwMcGOqUBN3fmJYqgUDFUljrGs3LbQokZOaFEM3EwHxLKvCTrrDKGt7ClwWZyasgHPyFubwUaLZ%2FZjtdrnfZ8cVgHWpk7cFZUD8Ipq%2FJ%2BRNKzHYyrrLH8EeTM02zUfET%2B%2BXYrixbUKNXLObCqWLFR0SRvgysvFfu3Kxkf0TlGTp4F%2Bq4p4nO4afckCVMTIsksQuijEunRp8g1MBApVGoA0ArEBfwnNj%2FYzxbc&X-Amz-Signature=e51628c84a404cd6fe6e9a9e14b48f3c5ec0c1f965c0e130a093622257f34ab9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

