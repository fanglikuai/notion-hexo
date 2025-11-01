---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHMN7VZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDCggtv5zDkDV0bA8Z5RkLMEm%2FQKexncE6swtvq8bmaJwIhAJXprM9MtF0BTUKTIadyolOYS4F4i%2Fwq8zHnlI7NTLM1Kv8DCDUQABoMNjM3NDIzMTgzODA1Igxj3w3aM5IJhzJmGg4q3AOP3L%2FBAo5zeGUMwTQIxowtoi8oj%2B98zDS3dtc5h2StVLwLgHpgXizmAkHpvZYyfGICSmEoh%2BAOBP2dhmfCYsNfQ2PKq4JAvIGtFppGydJBdafcVOBKu%2F2xKgFFGi22asnbngA%2B6Q4Svf3nK6CyMzt3JAEnVpxlwVGO9LREDIdvj5g56P7N8dTDJZIFZIT9DvIgtlhf9%2Fr10f57VTWktCCu65Ig9TXbsxwMVbuNR77nCbwI3aD%2FbEsQCEQtGZiSjznKnUINC65UEU8sKz%2FHvMBWi8xUlnItZGbEKpmw3xotdEktOe2GExnPeWLTNyNuQQ%2FewBH68cvsbwV0Om8S3YEhsgvebPfVop8G7oNkjtJAiLp1%2BygiDqrSDkWvRkRtColuWpbWtl8aaiolu1lWYrpoatB8Qsj2On%2Bben8Scxe7imTEJB%2BDykAoRn%2BKjszUv54TS8Bba07oCzjRfikObhzLngQMIuf8Dgo9qkwt9UvpQy%2FZTehfxRhSgoQCo95qACktnq4U3DTS3N2eOjYtKEqf0PrLZRExbeAljT%2B%2BTUYCqHxM6W8ADl1rRgJPuTF9SmSMmKw475UwR8Coos%2F6cxe3YuLkY0cHibSpUnAQ1%2BbRT1bSZreZMcmn8j%2F2RDDJw5nIBjqkAaqMRNWSB8JSpmQ79nT5IgFIz0Cw1ZtVMwJxGQzc34pEPvc7%2BAFM35gpWyEU1OCprcMHsQUmjlZy9Il71wmrKMy382vFfDuRhgmqufeLNvLLojScCKANSA%2FpOrrXGShGBE8qHjvWz1MAwtGJfa%2BJ9tFNg1RLJwbDACVTnjqIAyz%2FfjFDC3vEx3lnE7%2FAhZJu%2FRHmRd1YCvbn%2BSXWSg1Z4A3tarua&X-Amz-Signature=85dbfc8f31c2b054326aaa7d30337c9287a808d0d768355c32cb3190b962a7b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

