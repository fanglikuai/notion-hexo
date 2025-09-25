---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UZH2JZR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBbzGFtsHj6wjVupINuzSlIiYeM5krY2T4cWsThVMAmQIgWkFtHtHOqJAwwoXuHmTQdmHWfmd5DMw2qf74vE2VgN8q%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDNocZGyDL2rpK2fHsSrcA%2FJHKhMy9imEL%2FFjs8soZThKZHwvv%2Bkthh%2BEvwAhEqnIJ6CMl03TcnXi%2BNchWQm6WQjctS2BMi6P1PwNaIoZTeXwBd4RgXjToYbSwJBr00Z80S4Ai02gIvrz8gOugRO2xnz2%2FrKthXfUC6FYc08JvKEIvNuUxDOdlKl4R%2FEWPJA5gp0wu2xfcxCRJUoOi7ECztMa9msE%2F7g%2Fql0HEKNZqL9nqZCEsvt8JXBkNzdGE1t5dfiGUBf4mlzbKnK7drcaPq0mpaC17vFGAp%2FWdOFwyKFJfehMvljJIjYDte8KXb0D9bC%2B%2BqGOVG18ZTI3wl0%2BHevE27IrRrriHrYXd0sl1a8vj1KG3IAhkkUdb58px84cdLApY0CQz8fX66ofxUTzP5Qz9rbjcDibq6q0hdQxZEFLpWvt4q7QeIbxMoL1YO25UoSk52GLI46mPGaUuvfqZZ52o24cW4FKRPSTfySZZ1GW%2BQGZDfabCPBmPfwpAuykJxdtheRnHPorWfxRsD%2BN8vxGXSdmOFZ258xGids6uIPReEehTVo7s2eOvzc1E9ZGO8RpVcKVlvh2Nm9YyBsK%2BPkPff3%2BeIbr0P956I4TAHPpPV%2BNq2XfA8iZqKf8X3z6M%2BWoouMdangCVJpIMNGq0sYGOqUBNe%2Fwp7zQJ7EboIAhkZbMUjg9iPyqkPLajy5j8TIOpEkSkKin5Cu5n5caa5wJoSxvW67QzkvcsxcLX2pH9%2BE3lSkper6h352tJHYNm0a%2Fv6yJJNoFxj514TV3Ko%2BMOk7t9dN7GA3mO7uaxq9qcrlWtEJ%2Bv9AIGk%2FgPyFul9SVUdB%2BKEF6TNuuk2QaubNFKlKZQSTo21Oo12JCkFtIAtAJ2Gfs3DEk&X-Amz-Signature=049502e03e87ea16dcc082663b94f65fabb6b5c1cc0822c6a19db2e9869d3eb0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

