---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3LVLBNK%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJGMEQCIHX%2FXFSPLdv31jO2mMpRohfKwMBwLBTvf8gN3Viu1DM7AiAaM724xRdpOwBW3paktxPsBnLqFijfiDVATCjLdMH0oiqIBAjH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjseqRFs5Co3jr3oKtwD20SxAhMG%2FKidR06AopcF8zf8VJNEO84w1vrulacry54G1cOcGE%2BRgrpncDjIpPWbWTooId9mCvfxDaD4w8oyldCgGcJwYhArPgcHNgpWKLJ7N3IKw1CkVCIs6NWRcM1iux5k%2BiQ2xtB3Zt4yF%2FieO3Cva5DwdWTLSnZnAxjLum1ReqASShLpDr430YZxBco3vYqs5Seut%2Fui9qtf07eDQbeK9%2BKuClhidbxIGLeBel%2BtvqUoqnrW63F%2FT3fxkBg2LrXx33gZYnYWE9LC9FKYgPed3nEVZW5nQwtq6zhoF%2B%2FhbSxjz6fe1P7nwfUkl0CTUTqCjnReBs0EtOozPTnKmi74O%2FfMsF4NBJxtWZss%2FYNG4%2FLusDDEi6Q1jh4aDcg4J2%2B7RURV3HMTi4ymkFYTZ5xopnf%2B6MEBQI8T%2F4aP%2Fe2747DW%2FD5UtqtOUsCJS8S8q8FVJK9gCsaa376xuYgfuiWqUZUQ0KcHJKxgoWS6b0EzVn4D9%2FXlpvhqMB%2B0urtJU7dkIJzsnKeolg33%2BKfpVIsMohRBCbG6x21RQIfp9bdYMfkBwR9iIyJ4KhQ8VZRUGvq%2FPICGEtH7SUgNaKhR1PR9YCoQT%2BhwuWGM7mKILuPFkW2Q%2B%2B6o7Z7GB5Mw19rmxgY6pgG3kcCyJnMRANXD6xf26xkuDUiY%2FB0F%2B0pCc8TqAUA8%2B6SA4aWBPMv1dd5PYxfoA6IGX9SRuCLrAAD0GiU8x%2BgG1LubCGzfwfEWeoDZ3WXGIMO5fSLMQhsuoS%2Bkzcrv6VZ6gr6Y6C%2B8DaLra1%2BtQi6VFzwdaQFclKDU5A8gkIna%2BSqD%2Bqrw1UzZliNGdwTmpX2eRHU%2F54iuAkN3ZksmXV35osSGn%2FGl&X-Amz-Signature=db8d5cdec2fe31b9f1ea950d396a77094f063dd83ca3c8716bda1058b0b2deaf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

