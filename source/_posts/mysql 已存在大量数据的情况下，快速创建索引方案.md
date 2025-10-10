---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAOSTC5Q%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQC0PoOTundYwI5qc0d%2B1zBcFH0ci4XpMnpmceV8%2Bbp0iAIgXpbZMYwa%2BLUD0903kOO3BvCZQz0bQZljD3XiqJHEs7MqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2Fj1duEUBTypg1LuircA9U6LlXPz%2B6E8A7edILlvXMwG2AcSoFbEKKWYRhubwhNyBFBx%2BndWTATRARZ0zHHv5lkm1OJbRK%2FhK9H9pgB5aB0EfjGYyG%2FOOdzq8TOK55gqpyusadFwlSz0EGtymA5rIuvEMKrL77Vr%2BhtG9RISvG6bm6DUuaNUBezS142SbVmUmKUyyRfozUeJ4ObyuRI%2FW%2BIw8eg%2Fgu1GzQKgC5yDYamwJstDzvfSAP8ji9DrFjw5mJU6T1lYCvqpjWCYjPNCWD80icq7KpAm7OI%2BtbzM6RJstAN%2FHz8Ua6RB%2F4FeKPRvPjo0ErxsJweFAFvFDTOwHHJM3n7jE8EylE41VnRwZLaNSrfUClyp%2B%2B%2BZIZ0JDlyUyMCTWqr%2F6oVtnhmiPfblfIfsXNDjBszgTsCoa83xnWe9MEzeufQbPdmtsErq8RNr3dz4Nf58aoDCHT0u6d1e1pEtmWofGcLAt3tRt80dyRY1Y5Rh3kQBqp%2FN%2BaxKLDvbpfO0KNfQXZYIqZEF64aY59OdrYnHUh7oMIZxijc5CiIjKcYFaHRwnIt4HmzdRBpDcMrlUtW0ZtRXFzkvSkRs412UPlGcIU0O9KAMQxmXIjWpz9ZBR4zxJJ69nnxfZbMHF0m8aTszR4P%2FEAlMJGApscGOqUBgyV4UTdAPDys8NIedTDaeOzGeCMUVnPzjQ5HRZQvnTTeaHn9IqXWRHxZE7ICkOo4CWiQOgjt9Afp7nVxw3dgPUPPXtU8B8qMdyuTFjToBXFSKLcpDA2oiV6zORoTYpgzt65WLdcXCjo%2FJgUAQtdo1KjTCf0080TXAxfDDxVkBg2uIVdemoQcMuIxpprnXqZfvDyB08c5%2BOpLoFRU0b9j1VuwNE8m&X-Amz-Signature=d3329715da95345072c12ccf407fbb4b7444f052e57daf8849f8361b30cb8667&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

