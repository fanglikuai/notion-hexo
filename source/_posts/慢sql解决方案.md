---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RUK7COD%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDp2u5PFDH4fIxvhfwligAqLHaMiKNL7Z6ApWTyRSTo5AiEA7Kj9XSUWGYy51DUHcP4CIb7as60EbvPrj%2BDcKbHh89gq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDBwvfM3Kpf5SXm%2B%2FSCrcA5auXnkByyc%2BKIhhkulNKONLmEkzXiCy6w0Eo6zdA4XlpIBXQjE96dBuNbPT194mmK112DJOUXraip085k07KryHEY%2BG%2FqKuy%2Fpl0hf6ycaklWV8EZv%2BXRatHkZ1pN%2FTRdglKxQG6mN7k9rouJFvVWBKslJW7mpUO%2B2XK3QU4SovakmDHz6KPy63ta5hEKhayFl712v8rmxwDGXIjzxv2FxtYAYuAfdsMS2XLWT321HSU1%2FsjFuQlC2yqk6jINq0EukleaBl6TCxV3CR%2Bu%2FQt%2B%2BiRvkz%2BjSYyit3f5D3o1uzLnQG7WKX9cOJxDWt%2FZJQbeFvU%2BTjwEnx0HJYdLf3VrgyECSa7btWMJJARjmTsS0pbOkEPh8c9BJeze1QGprqQpAozkkRcc6shOhLgck3bP1R76Q6n5jI3s7LdSHZsUafjs6YP4O4gFclIWz02hD3lUsEbaCkLbBfaODP66gp0mJwAUozIR3ztsohUYZMBD3X8VuXXMxPU4bR6HZbDIy2RcknkaFU3iForpknGMU5JnZi5pNWM%2FMrkft4ZMA8u64p4oVgSdHd33KEBMhuW4%2Bgmu0hiLTTWRmwwSeJuTkmI0SaVoeOpM3gM1Vem75aq8agAj%2F4DVNGGzSWYEIfMJrhhscGOqUBMHDem01W43O%2BNzB5aX4R93hD7GphM02LjpuiVO9WIBnaLrsllM%2FIgTfkrpnZZK4KRhuwo7IaqtWt%2BE2xNtn4w8nkH3meyiwEeg%2FcMYFw47yQ9lfHU0FlrzbaWlRBMNpkocqUJUu44oeKrJ1Dga7Ejx5CwKVMGcMkQma4J4YtB9Rnoxa7zZlGuaGaa3dcjh%2BU8XROW1iaW2%2Bna2T50Y%2FhxtrWpA5l&X-Amz-Signature=d52324731f2b8f53360f40c7eb5e2f8ec37a1f1d5ffe7bbf8e32b7a56226f379&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

