---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDPAESQT%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T031821Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAvhpKcBF7n2%2FEyOXeYo%2BuVUWSWHOXsN0Fp1%2BdKfeiknAiB0%2Faq9wQ9hzBkxB4rmZzoeBe%2FLVrB3NvVM5lEuU4jCyiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMK3BWlkWP%2BBDcl8ysKtwDD3v8EPofFg4OcgodaIE7sU1mHqukQ%2FXeTpgtyxnVLyUce31DJAh9njGmPafcNbjOyygvbGcJzp4Vvwk0hJfop%2BNco9RVjppOPvFqX6MPd%2FdnMCzUFikp5wzSI55TYPzy9nGq%2Bht7KPlXhOormu8g1vNwUO38BVzBG4r8qJEu3xgMK3EOfxVahjRmwOGwxOBInU%2FhU%2BeScyJb0%2Bx%2BL4ernDnfviXMNWC3iqCDzoWNr7JknO1btKzBIHRRucAxDOnZR%2BL%2FocimVkVsfmVzFlnjdK9P05Nc6iLNc%2F%2F90jXjvKsSqNImiLXiWGig%2FViD6ZR2FCL9md1ypny6eGRbwXf58OQbAoNbJfPioFPJbmObpEItYef%2BvkG5xfP3ki7xq9E%2B8sURyEi7YgsSzhsgiljyZmTn5OwkHsYD54yFvD%2FyXyqqAPXHhySkr6Pk31JGQ42LRCjJLH5KOW6fzag5N3Bp4UQYmAjlX2ZlauARfh7BJdI11xtCegr1v3E9WMiKKOsEZGIPXpjZsM0QBywA%2Fj%2B%2BldMhFntEsZzqvhpvBIiIDQwWJKU3T7I%2B5QY4WaUSivuUoz5gJMCQ53lbpABn8y8CXEMF9VoDIYERYiKT6U8NTph8CRdI0DW%2FUusbvucw0oKIzgY6pgFhCo7Q%2FPDiA8pkAuaGVG8tglLstIxmaN2sR9d8VQT9Ch58e8rKLrO2lZUkWFyGRfxOiBAuiUnTXKxI1sedCJT9h41fzO0E0TfMkgNApS%2FS5zYjV5efJmbgozrAu1ZwGV3n1LtW3fh6wMtbrHT%2BJ3QQoAeuahiMEkD6usPGv56qHMhiZfp9ZUzhUohm%2FTEzE3uMq5aGyKV5ajtj2UqX%2FV%2B1RovwPd%2Bk&X-Amz-Signature=2612a99e5369c7f7d65ebe9591a961fd73c7b5f9e301a6a37baf36fd1cfa7fd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

