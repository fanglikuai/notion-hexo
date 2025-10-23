---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOKUCCG7%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBeYYIFC57l5lDpXO69qvKLNoYJ8sHzJ%2BqteR6%2FlE5DBAiEAy%2F4i8lNsHNqnK3VYsG9KSuodSVB9Ww1BI3GHrehC9BMq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDDLeTRFy2SZWhDR%2BmircA4io6zABaaOxlJKr3iVa186SntoEA1RQUMVviNHCIZmVoVtwu2%2B0SRS4cRM%2FsgHJxJOKdbwP6auTYGXAm3rELODzFc1IQE3Qv4%2Bfin%2B2fv4xo08P5ABzXfHJBArgG9uf2vEvsv4pX4m473C3eca9w9pTh67sYSNhKQtvN%2FTp4gutGMS398lySY3%2F0k3MVcdLlaCWTF%2BNohBCltT%2FnXjrjLigL9oCPu6klBA5Pg2O7V9WbA1S5EAHBSeTyHQdNDwVgmewTQQEY8ewez%2Bo%2BHYN0khit751Y8j1yVuNVgPpFsdluGmVOeU4%2BOB7fLO15DpgCARTexff9AkSgIkML55kpaGTlZw4T5HmnIIKsAsSYOpY4uRl8NY9Ugh7%2B9sakaudbZpEVLsi7oJIDgdbS7MmeJ6aRQJ9Ubg0Lx1P7KDkg0LAr1%2FxsC4BRgqaFi%2FnDC8FbOheXm7PMhsQoAP4KIffIY4omFPK6TXXlR3Fcq62uRNtw3qw1cCrZX3sfKak9IB2V%2BN3jdBmwn5bkjuwCx3AQUSn0n9xaxG45ArsgmKRiRKBZNBuSvjWl6wCsoeBj7JIS22u7NlUHcJkNM2g9yVE9kz90LDM4FvCWQ3kjULr1R6m1Mrj%2BKeG%2BeSbigALMJLj6ccGOqUBWD4WIUGVr7%2BXvFCDgwYwtx2MtDlGdJNbpfPjZT7L%2B8p%2FRfKSghnMVW6nRpJaAzu3vEmdip6GaGWf%2BWi4nqLctsqduUNZcuOIN3tkCFcvr4tgsR3hoamcZaf6q0xdSnA6KCaxLnET1zmv6ouC4rdvrDrWjrZGGFCvicA4NO1JfCOPNsQ0BZldzvjy2WucoFIA80%2BKUM8i%2FDP4ar%2FomPegqSn86d2P&X-Amz-Signature=ac8cce743d1e70d980ae78e49bf0acebf6b951dba12385ea6d34520f76e1662c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

