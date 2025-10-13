---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632JDFGEU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSxauumrTXY58aat6UudRvqwopD9eIsI4ObwwGtW%2Bs4AIgL%2BnGxgdoiXqQXyc5cHQkwDitjB6l3pu42vE5ThGyvxkq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDAru7WgmOav7mWNPfSrcA%2B5%2BIW%2F6i8IxSYspL7wD85cNvM4FSG1j5xmIKIr2DhtXjVjd62Ftb%2FMSEGewLoy%2BC%2BGLPSN6qWXscQMPEq1z%2FV5lShmQPtRPlbQIfFh9ruh4a%2F6IXJsw2c3NIJP333B6%2FuJ%2BIEWYJRltAsQDV9xcUXKZAhTOP%2BlYujbwfmCGrdlXQivMCh1ZYwWkIDD5S0CSHnQcw4fhvduCW2UKmxaU1ki2SvMDs9j4GQ%2FlpXIioAE0ije1wFgX7VlAaYruqYyVtPcYkcSkpTeYgqlEUMzFkR6%2B%2FGEiaUlzgkT47tU9qxh2XwrX9d2DmarWkKmITn5MqHIhkEzHkMLoedbnY%2Bu4zPKtSQL517P%2F5g2TGqxn0mPuwoPc1fN93aRnn0K9s2ELNZsitqtVnb%2FmXyBnxJKLKcnwksR1pCi1bfN3LU7332fILwuWR1Qbf4e3ZrAZQjXaftdmmCADFBJcI3uH%2BtZ7IOGT%2FTNSqSkDUSQq82cBtaVZu8uN7G6VOeJuPxHxEkSUe5lRqJmot8Yx5B8QaOPJPuQHdLWPJEx3%2BwwICAcEE%2BuzuARuBgWvmn3zXM76hA0r4U%2F48J4YRhfaORetsctoqNL8%2F4P3qH7X90fp7iH6cM0ZfCYMRXtH%2F5NmMV9rMLjRtMcGOqUBIr%2F9eFOT8HWMQfPkHkFyznTFEGdryahpoh7iFAAlNHZ7JIo%2BoagmkZF21tp1eMYICSLUBwklx7ARUpAjn2anf%2B4ZLUSWrU68Sa3ey8d2sqj5F8rYDtvowEeJ1lHrhOpyzsyAY8ssKqsf58nJJEHdi%2FkKIVk5TdcovX6TGDraf6sLiQAnwiYxjNZw9VV5Z%2BMnpMpouYHo44SoTx28Gc1AHAAHQ%2FFn&X-Amz-Signature=8dd6550b8cabab303b4f3e08e594e7a96aa5d8599a4a061bcfb3eedf40bd839a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

