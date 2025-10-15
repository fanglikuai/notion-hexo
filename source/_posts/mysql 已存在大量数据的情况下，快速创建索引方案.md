---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMCF6LGI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2WM5pDNlm3BFIcGA0INBjy6IiIdGsi%2BUHqBnhfho6mAIhAMn6IyTpWdxU%2B%2Fis4eOUmxcrAm%2BSOL2DGpSQcaIT9NQTKv8DCGsQABoMNjM3NDIzMTgzODA1Igw%2F8Bx02FOQfXrlQGMq3ANiKlChBrckFM5Ognc9NqQB5UFRozEAkj5BQDz8yafJr5CAtN3t49GKZ70ffgIdjxNzaVyzBvA%2FtGY4uVAvtiFZCgBCy190sa7v2i8WWxf6Zrlruj82WROMDIH7zYnOd67gcA%2FBwvEIMhBv5Wh2mxu337eo7DVI9GI%2BO5XffeuUDTAkphFCiAln6wdRA%2Bn1F9GL9acmJDvXLdm1kPEP%2B7yuZcK2PQKhdA7fLqx5f7YkzL%2FbpEUv5aSzVSF6w199VWVu6jTP1ewdz85kNFdf06oMzoFe2OMhPlERI6vnMxxVApEkKI4j2fqMBq1ggzAMTegqVFCwYmehQUoWLU%2FAYGv2CEXYIo76lvuVr3phDGpcT4Y9%2B7O08IJh6XSSBK9WyfZpWXZ1t9iAyhcYBfrnLY13AoAiIjv9wP52LlTIz%2BiYcpS1cQaJU6pd0PchJOEqMUvHeu21raeCg0rfvffSvO1dg5hbFqxz1Qx2zdEz1GOmr9ICdIUjTx6QceZWq2n%2FHF5X6Aisi0yqMvy88Ns%2Fs6B94lTMB1MYWX0aoJOL2yKximINYr3hTzhG5uZlIQjVAB8RZVYmWw5cQMHUAg5%2Bw5%2FbZExsudFIRvEdfaeyUUyCPluGYZYUXZeg40qmLDCHibzHBjqkAePG10imlOd%2FgthToLhgmVcScBp6rIMVFVVq4ohZnlQb5MFTvkUYOr96tNJ6of96m30%2Bi8aDZ1LCB19yirsHfpZ0t%2FzQYFh9p8kFZ89XOoWpzacg5fyjG8EBnrhCxiifZgVcuxhjIFWFSZGUx%2FDqwFo8BelvBsrF56j0XcO2iTJEQQ66TPmilJ9MWmlLJ737e%2FHyac2xtK5vO4uIjHqLCRvWB5rn&X-Amz-Signature=37bf1a2584c3190ecf53d3c11d539259ee57a7c78750ecdc09ce6417ad0be17c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

