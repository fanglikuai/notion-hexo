---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XEJ2QJTL%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3L7%2B71515DZxueFKLhNE8F0ROeGXXwZh%2FC2MG0NveNwIgKTXwF8Kj1wBP8FQyei1zZSiUDQpRYzcgjaYxlmGumYkqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL7jnbKe9m4cCWsNEyrcA%2B3W4OVrbr%2FzirVjweOZpMNbDz2madc8aZEKX5LsANC6wpLd1o9uMHZAdPa4nHsWrB5%2Bz3tNLxAXntwuX%2FBJ46KPc7dgXsLbMS%2Bb2yrYHfhPWCtEisH%2FMjnda7okaDY%2B4A77BKaRTpQWq04M7R2mSgB4PTUSzAiO5cvgS%2Fm1uSSrxhAt3%2FZzXbJqXhbaqJZSq%2Bp6ysGeGcTaK0ti0PXH9FXeaX8fSFzR1GqeLONqNJqP2luSMlPmiUlstb83Ww3u2sHEyGaQePEPvddSn4jbXgs9HmuU8iYybJpTWf6T0ppKnPfMSIyjOs%2BeO1sK3mXSNbJpH80V876gZ%2Bxm%2Bbu8KgEISUfimgZRdGNTwCD8zxECk4FJHIew6lYBq9SXZ15NXc3%2FBSxIxHfzF7N8ONkcbZzHFb%2BKXk0KqUpYmtrl7W33V0y7DlxeO34IpJaIYCqgv0GfNcGYiZOpo8zcXuseouftCoJzeBUhPEFDixROfrdE7FaWn9hF4WoT1q4gYSxoMutyNS0KGY5OtCas7l%2FjuJb3d0%2FpROwdKk7qxcRwuG6mLvuiwWEHINzlhSW3cmO88akcK71VINlfODIidnI0RtjYV3ISQyFIu0Ur8xFSozfJFD0nKIGALgL%2FHf2fMNSFrMgGOqUB9yTTAEQEwBpob9pMmWuuuHRTOPpneXgdbU120xrU%2FYeLTebcQ8V4GdZ%2Fj%2F2wLSADP5gSGeuZUQkHRUb73j4fuLqVBpEnL7wovXxoxuvqsPwcakMamDeTHEGNoEVANX7f%2FS%2FXHi1ajM2XjlZVDmV2IH7bJKNHHsrZphhVGPStpDfnt%2FY4qh%2BomXI3ziuxq4%2FxSsw52ZTAHY%2BfTLGjn6tonGJmQY6u&X-Amz-Signature=19eb2d4d1600af54798a0d3c23e0e7889d2d87fc192f659d5b7fd208c2225437&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

