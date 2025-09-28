---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PV2Q7LM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCGfcabprnaXWTE8WVMiS8BNnQkSXJSXXkdQ4pRCaE7LwIhAMnMgY2sujYoGNQV2nK8LXJVfPpDK1AukzCHIF%2B6vIZ%2FKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwaW1k7OKDRdGM7Bv0q3ANqdBuaGGjJ7iyNJNnzxaD2y%2B3j3J6%2FMXtfTzWLpxlVItaEth%2Fi7GYVoECebd3iAwYrRwAxzf7kbeGfgFmlcGX4UIDrLgR5M17UexnG2O19jlb7oJLAvkmznHD7V311EgCQMtU9eRw3LwOOZ6qCz5lf2aWFxzXAnIe6UbDZHYITtMrfrjHOrIiBwuxPYal18f4Y5VJyrJDbAc09d4lB%2B1IWaUbMlbNY8qcN0uI5DYHslzX%2BsL17mD82R33Hl80MCGJljeUJ5Rh2%2B93fse6rGBPHiJqOA2av08dFdYyzqVbbi5wMjnSviGg2uX%2BInlf0UEqofDnxJXqpelqI2sIIpfaamD5wLnXdntxWsauGw%2BD2pmntKtQUOd%2FhWkY3PGDOiIr6S4QjnMXQXhKT6FhCBvoV2yf0io7M96Cn8w1zSQ2sqbGBN8ymtQK64TvuskpOe87kE49%2B10NefuJUYORXjQgnZITcfcg47BPkbeL26qN4JKsVB%2Bf0e%2FQrf8WApcpG3LKa9bLBMvQ2PXBRKsc4h2zXzArVuUct09It5kp4iPcrll2PTiDS9VGfiQ3MFxvXdNIzE7WVfWO4kfTlvYXdtaB%2BDLPtMLqDTgyo7AulgsTbKoh9Qm9qDE7zZ7mpNTD%2BmeLGBjqkAURZim0qBYh5l1LsK3GVG%2B76X3z%2Fgay9hsjhWOOuscLMCtLI3eeMCSDJOaREz6xFDV%2FIfNcuJHSXm36M2cojfgJ%2FFqsHDucHKyfgdqf51DJsk9xzGQnE%2FBQm1b167K49I2tCgaXtYmItue7cqDIWwN%2Bh5xBTOwf8UTyLkzBAshYt5oxtX1lBCa61m48FKG6RZlpqH1lGyqQV%2BdWe0zWhaCTDxXja&X-Amz-Signature=8549e2210506d5fa359cd18b8a5cf53f6887321f24c5d340afa4444de7ecc917&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

