---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HQCTRFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T130046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJGMEQCIBmTeE26sWxwMHlygtQ7Cpme1tTHnZfhlT6gm6vO3G4cAiAzHWZooD2%2BRFDsHDxM6KArqIPtXFSua9SBWLfJHGITOyr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM%2B1JYgXo7Sdir8z7rKtwDXmzFIbR61hciv0X5of9FDC2QynqvSw3oHBblFvfcvGk4AfZP177aeJcbLPikxMb54xxBH97ToOQbeQP3w%2Bpv00ra2SKBnICt1qoGWCzuhaUMe8tlcmEpX8LtdJYXHt0pni6tOwhZ6ZHkPW0jRkCLNTJm16z6gyYbwJB7H2MGv6MFsr7GFIULo3FPyp21hlayfob%2Bz%2FDQjQXOwz3gYY2CR04JRxtxUc8uTdENkfzSX%2BYDtC67RfZt9Aym0c%2FUE3rU5iLC2%2FS6SXc%2Fz86lWK14cRCPJJbJszE83arAhcmEv7KYzgKnndSfs30PBq8GxwxivkoJ5n0q7CarruGAhaWC51FK%2FFaog9twNx0S0MJZYpdLyJCwbQq3jC76%2BQc56k5JJFXmFIMiePsFZxN%2BMqWof9KN8nJxqFpLH33JiauG8Od8EFv4nL1B%2BqbZ2tgPIMWqm57IT5URKLDQUlJVfnBl8sztHGtnc0I0UV4zAsQQyET6stRsnjAJrkltiTy%2BMLD555tqXNGd2fq9vsNVxRcAmIaD96lW6KHQGFenh%2BugTOc1dyMhK2VAMaICpS380yA0BY5ZNsJVzajXy6VeWLo9e%2BHiCXrzLRmk1urCysqc82qNZHv9r0cAW0CwtCEwt%2FjdxwY6pgG3gy5gtkEytqhe4WdWrow%2FdjYgePIKr%2BbER%2FIdQePftZ%2Bqu9ytF9bp1eoVV%2F%2Fke27IFFAtbISW5cDRVv%2Fode8XShyc3xSiNQVBg%2F%2Bfz4AorxtadpBsuG%2Bmoztcq7zCeL19FpUewnjXvdtfJn%2BQU4AyB%2FaezFJFnbWuLemWLFMhEMuXs6TYheyf214AyX8m1F0h8MkWYAft%2B8Xrk1wxu7gBM9vOmc4E&X-Amz-Signature=f2a738567c75a0f33f2ca32dabbb6ee7aa1b8b5a2f24281bf9f947ba6b2b0634&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

