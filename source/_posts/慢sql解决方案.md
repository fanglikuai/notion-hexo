---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLJNU5LJ%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T140106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCL9M0a0MivLdK0E%2B4RKzSeqfFBAytGnGZR8a%2BDnMChPgIgdZYA4a2PQp30nkyhL7gzXHo5pVKOIu%2BcDAgqW8oii7sq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDINGdPwDM%2B7pXpcN6ircA5Nib5%2Fgnl64b02o8EsBFMIhik4gF%2BdZyKGVIy7mmef6jf6f6hHfDKOlbFigeivaGWB04h4%2FdKmGL%2BK38AkZAFBH06grcawwBW7W80j8aHVRseHYjFQ7ZOVd3fdY7OEDKnQErncs%2B7nbjXP0c4biQk1vOST32QaDVdYsbR2SNnev5oZ14kUvoDxiozXvDlnsHPXw%2BKerEa26U%2FONOsVQ20LsmTTft0CqMjWfPqMcB3XIFUIU2dJv6kXoRPzM6%2BXr5zKwFvnOEe1%2BqzbiHwGVh8utRVAgsjK0fiFoOcRYpW%2BX2%2FJWdpTqyXwDDPWShVQcP8yIcjrA0et7UBvVMrQjXc%2FPuGdzk86onm3e9b%2Bm9O6EnWRXPjn1Diizm1m5JUeg1OwqJ%2FCI5U4NsNUyN8kWJVrgI9A%2FU7V6h76WQr74uwh6AOQoSGVlVZ8RlNnAJNRneIEM%2FMrcPJikuR%2Fz%2BQ8iIXGjGAoaLpLm3SbtfApNWjMOtdRNRoJBhnGViRyse3Z1rpRJBTM79B76Cftdi9f22EgZkcBfzf7dppFD%2FNEvkZJafO6aBLVQwlLgnCSIuC7Ytb3PNDpVlDYfzuFVUkly%2BRnIEd4Cu%2FX9DIL9SecnZAlVftUeaQPMwaNB7A9VMKvZ6McGOqUBYc6mPsA7wJdmnTgnp%2BBhglU3K5wITqgxyzUagvUBl78%2FNrFVAQz%2BVevVtDXIXg%2BXSf4ahoY6nk1ZkdIVG6U5aNEVaVw07sIwMuSjaT43eQ1urnSfsKIQ8WruhpgIexsNCZIh3%2F5lycNrt8Sjtcno9fFoXblQ2hOroNQhj7QAcbCAtBDeiX7ELlE6XoqURSTgb9YBUA8eU75utZkz%2FFrDMP1A1iy2&X-Amz-Signature=2e6a7819580d60849dc3bb74b47eb4f16e3018302d784d13a16ba8dd96bbb1ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

