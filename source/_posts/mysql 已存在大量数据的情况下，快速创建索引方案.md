---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJLHNN4W%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiH%2BD%2FUdpcMkn5GebDH8RmXIXzSEJrEydAMX2ieZDoCgIhAPoMf8fmgo2DFrUHVHfZc%2Blvz1Ts8X4BeJ9dhOw92rowKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4puKQNROmPvpcAAAq3AOfLj8c1XY%2BRJXcudAgHSaRYE%2F%2FEjEs2ELXRoazh0TewKJw%2BEUu9sWab1LbrAW5gTLlXfYtDBS3jVLTE7ggR6kfvwNV1bDuzXynUof%2FDatLNsaN6JiEtz7ZRlCP0yvl13sAzWYBF5ND5eucO043JFhISfkSqETXxSejodJlXgU0I%2FH3bssyCoSTW79BCsx2PgRvT8Hohta4seSV4HRWuT948Z9oet87lUFWqhqW7Qj3wgtSaHtXbt%2BIuaq%2BEaCWMwpRubDdVa0qMhaCB8iHq6%2FNHFQh8pCxRM8uoG1aOb2XTvuiiTdj7pcK1%2FUlAh5VzfMOORb7iwjIBCSO02ZelswZmZnHOQXSs35QuWShXmcFl%2BaaIvzv92XIoriWeav6v44BUB%2BaD7cPk5b443wghVK4%2FZ5gVwCPGaca2C1s6GLcPljebEvTIQL%2BGjRUkYe%2F6SD7Nf43StvVdwtYBIvYIiYHGLvGMzdpDlSBCw2t1MUzWPzxd7WUPWG9wKf88zilV5fWywZ1RH9IDIWrlN5Ejdt7KFEkZFshB7mhUYPbV7HWqIPoTOpj2F32dEfHqeCca7nMDtncz3%2FMp1p%2BPNIiRybv3XutU5e1ioijSuSKdTriGo7goPT1m9USXmXUWjCCurDIBjqkAcWWVFT33Fp1ApYi9sRS2Gr6LWJfwiMXmgaNUsOY6IGusL%2Fxp1PfZSwWFat5zv5u6gMfAt1advu%2BdrSWFP0%2FqXv1MbCwkSS03%2Bvs%2BIOvnFL5oNNr%2FVUt8dzB%2BwIzyIG44TcDSYEAdY4eBiKRBAEzfosFP9cYmHnDcFcCl17FOq45xUHlJ69AAOG4%2FYzOFJdXdsr%2F9%2FArL3NG%2F6LY%2F%2BTNV7cda1nK&X-Amz-Signature=b29ea1f6918f6bba92ea988454591e572afdea7617b836dbb8859940fa7ee37b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

