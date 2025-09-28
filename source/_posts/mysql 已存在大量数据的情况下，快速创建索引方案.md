---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPMNT4CG%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDTv3GEQ3VUPwE0S7eKQKqKJJpUbJvqox9nTd1McU7bAgIgWTT0yPqprtHRSN4xX25do2V6s5lwE%2BJog55hgznXM4sqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK%2BD%2BjFLtDsmn%2F8PircA3%2B0ObGOcma8IM3r3F5XreBC4Bi65QOuRyghgvPmmKivHbt9qh8XGIXZ9p%2BZzfME62rjpvlKYdZw9CO7mEDlszkVXJeRBtSpMc6h4aVavhKGoGIt%2B0KAIGZzk3YERe6l2c1dtmmhO5aLFWapjbBY0v%2BygrWiJmuvH2yFEFpMVs3YDNjWgg0iWvkLYC%2F5jJy26TtALj%2FF84beTt4VZ%2FkI8YRdr82ScOYMJeQvA4j1Qlbplu9mHduFfiHXF%2B08s3Thtq6EysV%2Fo993OleXrf4%2B%2B6Ui3Y0ZcAmabNh5Pbd6Cx46rZ40m7mRlDcA0tUCcpTi6HDIl4At%2B2wFKSXsWN5sXYeaT1Ih9orzmxdCoNg49tuimfWLhQ9pg2SK72xkzRTjYa0cm2pJpPRAu%2BUfj9rrB7ksWgFjpEyWs73Myyh5aaebLVPjYBb3or1OQYURuvguKeCocLINc6o59L7V1bg8f4MLQoiUYYOzDTfnQvXv%2FSo%2BSsrEKuROx3%2BzJvC%2BPjT5pujXcr135gNulBO6yHdDPf8zZ1X0106jZO%2FpIYnxW%2FA1mlP9y76gvqu9W9VMupaezQqVIjtXYFQvihvMGeA2eyNQImFTz9Lkxow4y2s7BTEj8Yvf2auTgm2nkiAIMNDX5cYGOqUB9UHUUUTJx0HWJ5t8%2Bv0tJ%2FlWeViz6Nnlg%2BfIoHOe0hkzLI6RZQh4OkUPVn97PcgOYHKYNUVWKm%2FUQnNbx5ov5JW4mJcya5JwcTvcbJh1tjJ59RI%2BWISJ0qTNQcG2zX0CrTGB%2FaFg31Yc30X04P4ZR0D2V%2BjxhzU1z%2Fq55W4t6UKqMZR%2BC9DSPDek0ZIrNLnr0N8cx13AkWg2ir8q%2FvNJ3JO9fgkC&X-Amz-Signature=727f1ab0e76d773d9ef83be86ae85ec81ce1b747626c5759e432eb663704cf0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

