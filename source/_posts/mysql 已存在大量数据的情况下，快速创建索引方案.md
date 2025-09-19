---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SV2TJGXB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCePKFNWJcCH7eTXLSYnkvLuLlJomyJtbVUEDCw7NX%2BhwIgdi8PdDsJBRsDVUrH3fBHsarkpZuptPZz3EYZ1F3AKlsqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBcqcEUAze6JLkdo%2ByrcA%2BqU4bJOcSfHfxosOLfevMmfN9Z542CliQoqFDtImCWwC8OTDHiLKnIvMY6yn808xpZJFCs7jD2LuiBAN4FDO1fyrDq6uDcyuZNBhWEgwdM0mqMh8wLFbeWyYxeKHUNhbIPFdz6fj99MoGfIlKBkkXimmzflK6eP1ka0n7iJfNIrSz2qImS0hHx%2B4W8YIK4gSpP124JncMZNmAnwcpLeyFR9FCJbCz11ZO2g4Ui2LLZMBvpsw6RGk5rhe9qG3KvCC6CapxjPQYAURbnLX7F9DsLHTliJHmrnQRSGRJDdnxcEi%2BEwjrIAaa7rAHTOjhbcS5bty0%2FTRtECIRWKF%2BWUw4yxnRJp%2F5MIXUGKAKr8iJZ4CzBQQEN4b%2FPQsIiugKWUUhdasTMjGqI7hkBaeLzWlrKVVrYU%2FMX7eC9xMFd8Dl8PpwGWvuIACdwM%2BL0cEoHPFoeIg0ss6YB5GNDUI%2BVVVxlVQbiGzHht4SGsX4WyyittnhE1%2FbHSW5xK7dGvZ0J8xUGiEHb0uI%2FkOznvmMqPpq%2B8bpQMoktam199IrhjVSNE32gt4yMiXJQ8MgqpBZDUMOCJNEHHb%2BrRibTINrkJyyurAFq%2FJUFj041bgoPqXrA8pzPFWyQ%2B7o2NyxYgMJDjtsYGOqUBiK8jtQOFWhIwm8yPK3KzyfoGI%2FdiAhCRZcoovF6vYvJloApEp5Sw181eDNt0WCDl5vw9a%2FqtUkEGIS9jckDVXWvvY%2BP%2FwHZRonOwV%2BVTnnIHhzWkAc%2F%2FHbfHkTMmAUfP397nqfAUTcaKRB4mffbVijQbpAdaH56eOUXZowUhFaC3lhIVDY2r2jbphSoA6qvdYpZ4Ab5ssU8obTwiAtY6mmFU3GZw&X-Amz-Signature=63fb0d9e0a6c124a63f08a8c2f7c06e8b6614fff6361f4d8bd25e089606b49d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

