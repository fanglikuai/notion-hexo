---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHV6EAGU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB3zxrdou64z7Oo%2FfBR0AXnoQm6qM3pnCVGAM%2BevKBIlAiA0vS6bp8R3BsSlgNBOPvVuBefw2qD5LFk0jRMidzjckyr%2FAwhuEAAaDDYzNzQyMzE4MzgwNSIMaG%2Bog8d6JlNT%2FPyDKtwD9OIFpBZxDmvO7A6JLPRfjteTUpncrwmFZy0xhmfTBQYc3dghOehBfvon3iFtTkbiGIrDhcrbo2R02KvmVMcMFU4PsCstpK%2FvfHb%2B5hNJ0%2FQkydYTTf5ny8p3UPU4NgeOdk94sTdBMPt9H5tKnhE4id9eNFUMe8bHIG15K7wLP3a%2B8T9ZzBK5FMEXTJOoPCzQNjjnqdmueFrCGh2pkToj9rkzKN%2F%2BYDVBs2D2edjgug%2F%2BlDHGFuyg%2BSe02CKevf5lBfY6VNjEIRSwFV6NnxzFLlCB%2Fh8cXOtJXDkTvjUL3WaUZKQrVjlgGqdc%2F1iOMCsrWJtvbRI4vtoKJF32NzlODE5BTDp2CjPW75GNyuItPmFBJeSJs21uoC7OAJLQIuEve6Vf8uvwPMlpKs9nlEautKw4JSisD1b3ujfWdoXQcHEXfLyL7qLWXT7fh4nxBOph%2BKPyblWep5wt4Ipt1q2o4smJTvh3mZEyVgwr9%2FgRVeW7CFk7xSLtVIaivPCmbMH2kVRHcCeZYLddw1Gh%2FM9czghgWGSxTiOreDA8FAPqJptSooYWFGtZrVoNRvCwUx2TZAngxg4GAdrKmg4lLzbxsweG29FDOqo4m3IO3yCq67oG1SNKPz%2Fvyi5ozZsw%2FobTxgY6pgFQo8Gaq8vqHI4WmAZdu0kgSM%2FmxMVpX24vtQDtPaZ%2Bwfd6bU5zCID%2F36yySf%2BOiw8W0snlXoFL%2BQMJOej40QgS%2BTfEyL5xpbqTS2vNhPyxYi1PIZQZDlm6%2ByyUuuxJRrZ8L1nnok0ILm2o2L5pQ%2FTlgsTnrorVPxivOzHFXZTa%2B0hwrBjK49WT%2FipQeGyo41WCEmfwzVTTuES9xcNlUfnscs%2BQidqP&X-Amz-Signature=00256765d09b2eb7df15a51dfbc108b38c9759e86020aa9f02068f7d3921b70d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

