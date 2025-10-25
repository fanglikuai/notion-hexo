---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FSQHBU2%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGJ1z9EWqxcGHLLGBqDhLUxLMXrMiwIpxqRfgLc%2BsvMxAiBjiVjQWzzRkMOS5C5ZzKqJOYtfzjDd8PWYs9qQxr1YQyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMNexi1HtTlhGuaPOFKtwDNJYwk8VrruTL4%2FJR9MFm%2FLBp4TrXhmJFQdo71TjyKw0P2AXx9Uvr%2BKkJo0JemqJtYV6QZHL4NzMMY9aP20vICThICCldvL3idPrINjrJsEQfw%2B1DhR7ZmruWyDjMD51ydIFXV%2Fzz9GPtgBsV6KYNcr%2BLWbRYUg3c4%2F4rGzLUUpZfGhgthfIqwiCU37%2F0yRBZS54ZKNEN9VLPpUrJX2oJpWyC1ouVU%2FeLgS70mH2uwO8A0dq1hVPQRK2bZYSb2E0Ab%2FqvK6P7CWMGS4QJgydtQUfV5Wr8Nxy9PN72WVphYppnjMnU0zS4Mh7n51vnmIJpJ5dqnqANZ2wZ49wTeSY9TMxWTMc%2F3yZ2QjqRLmaTnNVX1AqHnwF6YarMb3raaB4sk8z8TQqC3kNmFecxEIFOBUJkNEEb7fK4zGO6UrvlykFMP3LjUQpP8EcYqk82YdtGPNxwzdC0DWeUgBknODtNzj%2FKqt2di18AzVzs8ZQ71dHTg24%2Fy2b1b32TMhA3cPklq6mK5pRMsh6gsIvjEPwJ%2FkT7szFXMs8FsViSMwdjYUqh6TXnWEhGxPWmrFUr%2BvPhMNtqwiEoSbUWIqi4J%2F%2BP%2BTRkqrhwHAPurYeJK1ITXM1gWUQqpA4mDMNCQi0wjf7zxwY6pgGMIJ3N%2Bt1m1%2FpkUmXpx3zeivrAa3BYgZuLBv7TvHKpZuTJoDI0CXwhNu6lGb9IIro0tAimRtbAPTKm7DeppnJw17uXfFAM9jQDCRJGljNm6vx7PlHTZMh2GrYb2OK5YUx23jm2WZ6c0CvH%2BQShxMDchO%2Fg%2FYghoq96%2F%2B%2Fb3LZ%2F%2FWYDcE5SEr3XNkK81BQHARQR0ksDyHLwkzbEJDyTiS867tB2Bv1%2B&X-Amz-Signature=cf6921d267ee0579f637bd8a049300fb5677ae876dd4a3144e892d74ecfb7552&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

