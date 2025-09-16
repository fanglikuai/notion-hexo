---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5BUPNMY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T155517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBSNMuLAV1WXWaSXE9vSdWHJQEvIVJsOdHpW6xgN5ISJAiAfMCOulbIRa26FB%2FTjGGrGfgGEFZpM%2Fzz7uEUIhmYnASqIBAiQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUP3kmBQGida1GHtjKtwDTtu3MAZzV1kZkqKickDfSl29RCFopbkh73FWGZPQExln7Go933fq8Bi9gGtcaX0zaN505nZ8MQFe7wsnbdCUGLeFgWGJymUPhBr7qByHX1238PCz5tmM3PHbXmGOasUikxVo%2BlHdoYgJlFJptyr8gGGItjg97Fz6mpoeBQl8lQmJ29Ta0Ob%2FUAwL%2B%2Fvbiw6j6JbTcD0xhYoDZjiX3SFeaJR1op8WoiOUKmL3IMKIA%2BC8xx4Fg%2BJIm83Oj%2BOENolnNRlfi1%2Bt7BUiiiwhSu4U%2FUqstPHBxzoV8jzaSGMRU22cuZPEdPUMnKwCzGUiE7GuFwoXqlDw9pf8Q1cw90LFl6RI12aecUDU3LQgPVRA60TMihYu2YLJZnAK1iNd4dunRmuRIP37hTbgaWGI2Dy0KJYMRIVGb%2BUfIVA787N6kbN1tGVoqlaWcamoo4aQcyFG6IaCJYAZuXmzXWH8OT1fkcgWbC99iQDNztrL6uE97QNks8qp14DOlE%2FWsy3z0o3%2BOiaypNYvArp6mVwxbala6IuwJ8G4g8eaHpQtHmSpl8ZqVz4r%2FW40d6wGFg04HcLyqvgoSaxcyL2e97Ws4Vuuh%2Bq3kqx4KmAfPoxDFPdf1DdJ72DXYCGbrqoP%2FFEwsuilxgY6pgEn2d7%2Fusx17eJRT7soIC8REVKjRVdnIB2xfieSFxxQR4TdtBJvtMaQWFFqrclsE0Hy8LRxaV40GFd8Fs3Iztw2Et9X6%2Fe%2FIZT3a3b2O%2BFlF1GsJ8DnugTkS1l8SBInd63qD5whBt9cla%2BhCuspv1UZ2Eus2oqtyY6JPmGQnUaZ%2FzF2t6ZlnoS1bJ1MEh6MqlgPDdAhLdtmO7KVoC2XDfwdmFp9YEBT&X-Amz-Signature=a767f3927c20b83064a9bab73f4784498c49a96c677df055f6168fd5c1a75706&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

