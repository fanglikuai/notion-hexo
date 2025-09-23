---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SFUPPID%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE51cv91Bs2CPbPNpka1SQex7xJQRrmDz78xJtKQpekTAiEA9EiGW0xepoOjPAj3Joth2AUjrj2fInMzVwfS3af90hIq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDMJck%2BOVjunfDFhw4ircA0wQYsJJPYBDn%2F9h0MG7cVqRnvcXx833wCN4XAun5fQJqadcDXFmnf%2FtPFpnTefEhh4%2B33KzCdSaQ5OKEhoNSzjltqDbgir8643E2E4v4NnrPghPBTRBk7YjhLvcvltNDcc9B6Ilm0kV2Ytgbt%2B3z3V1RxPzLsr2ltrkwvKlmr0EhuB7PhfKiUnBdTQY49PlAafr8M%2BD75km8bZ8wSrVStg2QGyIAH5qdOQmKwPAqtj8s3rRbuGjsciIaVwZIiGf9c6hKQpCv0dx8NbM792T8Zw0qi6x3LEglq9T15nvitgTXZozNuR%2FaPDLCX8sJtd7bbgjPuBBpOjlfLkpv7gcKcdFQ4HkSKnDvLf5UuzeB9A3Bp9DK8V1QuEt2P1JzEmKhfNBQL0e7T6Csw08%2B13NiP0qgleU2FTmryDyjUxD8VTYdtE1tij12JdJ7Hhh7nkxZs4KbF8V8Ols6HOEThoYVV4Rr3smPw8K9%2BaLrNNjBKb9u0MUp0IoBHc39YZXo%2FuCgCLUmTi4%2Fe2Tfc3rXfrdaDo2N5B2Ie9pZCPggkpPZ6POGttiVXkrwPGW5GLZUNxax8vwpv8U763Ds%2FWxG%2BZkGVd6TuGe1vEjBtNnZfOo85FAf2yqP%2Bh7gFhervWYMMaTysYGOqUBKKER%2F0QO9S8a8H2LvJsgLNM%2FofxX4R41Kno%2FlsQkWGDRcFOpmmKs%2BxyNSs7nhpDgUEX8H%2BxFcgicQu%2BjySZYmcTHMwqG4oHaqGG2hIfQPn6cpmZRz9%2BQ9kGc1ZMV9VxnGIwSvwDzpKpzWFmB%2FzneJH%2B%2BcTSqmirA%2F94eakqojdV2Cw6HMUgbLtXY%2FGRw9CEKwFKPmuOhkTMu2LNgg7omsYKXwoVa&X-Amz-Signature=d9818dba0ccc61fd07c323ee6ac97d53f37880b6a3a09509d29caa0029e6c011&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

