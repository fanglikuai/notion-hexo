---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCQOOTKV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BQL3vvAtjlAQDdWEVTxfFKJ%2BeZ3%2FD2s1s4vA7I9CKKAiBttWJ2PPNd%2B3hMYMWKeH5NCnV88FdDhyDOcbDCPXbkPyqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjnATRKk94dUgn%2FRyKtwD%2F91HQK9JDIPOnTDAyuVRfl%2B%2BRhG0TFz3L%2FlaJfLEfSIuuz4ny6VFQnujCmehQkn2Evej%2FU7SxVf4yK1J4HPE7ZQ9e9%2B21zqS6ULIeW6hEA4CBpMUtxLZvG%2BrH6Zb7N5FzPqW8kwtwQsfGIfohi8YnRWddMiICZleTNl6V9eUt50AW7cC8MhxhajlwRLk%2B%2BgeX17FtbX1CVk3zvA%2BHwVMjNLFuJe%2FsopS03LEJt7t5SBUorbNI3SVeV7D99Zg9hagruZpbeu4cM7Psk8tm8I676L4ahli4o2kojJwQSShp%2Fv3o2%2FHrsA9JMKifB2eGg5EitEnHpgXFrZa1JEiC7Fevpv16raQS2%2FGcVwYX2pyxeZRlFuDBzixHgG1L166uO8K1Z3YINvSAWCXREOBOkekVtbBAE16he1lDvz0kZ%2BE47gt5OVy0QLHI58nQQy5dc9npbZCGM%2FPasLkmuMz8N46nChPnZKViNS6qBVMNwaufwv7n5CT%2B1bYX9lRN4uDbhwjayn4ytLzKtY5sW9hLBvAMe%2Blo5dO0RvGdyS3P60mzWPSXh4ZLp4rhRY7iLCOHmx%2FAGmkM3qTTg165lTIG1fM8uZKqpg%2BsNDscxIuQtHrf1aF8EHVoNiCXaO%2Bm%2FQwgv3IxwY6pgHXas08hAUq2zorG9AcXiPoSeYc3L6Ys1uPOQroFDxLi%2F0eSkhDKKCSTCAKuh3pnzVanlyH%2FRKfu6GvwgghOS8iCEEU%2BJHUi9b%2BfTNiBectboM5PBPc4HLCysot7rLr93i36Jol4gwe%2Bd%2Bd9WYfz14Flrd%2FpMJZbubLb4%2BFvjmAKLrDIFm9J%2B0%2F6TiQ6hnEioy8TDUun3g42XSk6CE8NlUVm7Tu1YWZ&X-Amz-Signature=ba113952fdab770966f4dd3aaaf0fb41fb33eadc27149909a9a79a3cfa2ec1d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

