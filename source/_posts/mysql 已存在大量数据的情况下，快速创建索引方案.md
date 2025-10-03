---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RDC3MFB%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGUZyMGcDQ6D%2B1XyK%2FbIrbq6FYdtczM1%2BnP7TcF8B07gIgGKvPvcPV%2FUZYWxFlH%2FmNsbOddkJSUWDlfyNbA3Nbs90q%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDKn2pPwwCbhiCL9N4yrcA3v5liUcLNj%2FS2KZyd7Jy6ApCVXRYq5CZLMdsaZXBsivetp3Gm%2B5kfWj2fT57e80ewyuqfBFfCXr8jhE3dnQ4YE6PRU9ebKsPZQF0EzbINIy7q1PjsZvSKag%2FKp1GnDbbCMuG1cO8e9io8Lnf%2Fuf2cwF0oRFx82omo4COTUc2zMtJqsCGW7TkgupcO4MpkAe4jAeIXV91Qlcp5ZUKQGqDP%2FjrZpmUI1EozWi04G6S%2B5egCeD2r4GAwTNKn1RV1APAjLaewrc1unnUXcEK%2BGyMxptipJ4ORB2%2B8gQal1aLfs3n2Dk0nlWSJnKz%2B02b5gtV76vg3YVYmV3BepvxoiurGUR4ACd7r4MkI2joWY2%2FN5ez9jfSBVHTe7cAOvo6mjQ6i5YmY0FH32fQJt%2FRdGxfNtrIMCdwQm1mwaIprUaW02V8IviY0yKtNZTyhm20OciR011IoXLJkvuuqKSKSZloMeVqah9SMJYlNzzOIB6djq9T8zkSDxi2jTWFbt3qDdv5FhtwglaUBE2izn4oohz9L%2FmJFw1uUhys%2B7TNlUY64H4c%2FwdshaBp0b70bXfpDZPllF6uHWlW08KT4lFbMzjL2QUF8BbrLB2RO1X2X79QY2r0yVZcZn7xxCWhs0yMKbx%2FsYGOqUBUb6d3s3YQy9zcxdUFMeRGLmJtScUoqbXkpEInJYz9T6aMToEUDnU1iecPFgPvoTzCCULT87hRMDAOvCNE5xIWMAqayfRj4EjEjJoLl6FAxKSKdjhtPKwOYvoI0478zgGHKVnSaKP3AjByTS9BdyNPWKJXCRcY2E7OgrsumOvhd9WKNVa6YOaCO7xIyTEgaA7wqpx1OhPYed3T%2FHx4YCPGvyd%2FMC%2B&X-Amz-Signature=5d09084594cfe252c3b71f40526984b334a2764d64a427e421166230009623f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

