---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRGLFFHX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIEYYjy0cXdW%2BEMXpvA3sJSceknnvfnQOZMG0IMOLaGIiAiEA0UAo4nkWST4iOj1zTkzT1oH28s7fazG8SuB%2B1Arh9cYq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDBQFIKQ06QxYFHTzHircA0261kYKW%2FVx5HvSddeGLu%2BHRXQRzPGgVeS6TbSIUmVSf12UklR2o9uMnGw3hNAi5z8w3g65HDFWmqbXWQh%2FbzdW4POH9R1dErXk9m9dFySLci7qTj5RoPUtO7KEQtyHqGvtXW%2BFzGYvnvLHLAH4vlpoSNwUYPry4yDFzYfe5XCrjrYDodYhBYm2988w0YuxD3vzAHWcOET9tr4qx5J0QXStzTiB9hgBOeCDEX2nd9L33j3tj9f7jMlu1NpQk%2FU%2FbBHppXfYC7eZWWiZ%2BklGa72ueik9iAgZ3uV%2FYTOV7hk3QVTJjXORRRxN0Z55BHaSZ%2BKn9AYwIEQ44MF%2Fy4YmmgXxvllpnbthHh9h23KUIk%2BO9SV9h4%2Bz9De0xa5xqfFIilePoaSMQbb3ZOaNAGhDJEr2tBSGi1uPsvcHsFrbFqKJaF9LyD%2F1X8V6PfCY6C68f3PnG0XKF8qYkHwGdD5%2B8Sc5TOZONGSIkGJyORTY59Uh%2BPg7MrsyrfRuftq2%2FmtO9AZQsDigYCLTTO4BmM0l9J0LK8u9amE6Wd%2B9RHhreTcuCbF825hbp8tCYdylUNXgT0p7Y115M6JwUG5OL52kW%2BN9Sopkq1EP6tklzGBd63j3obrPY2q9Rb4PUIluML22x8gGOqUBxlWv%2Bojxl5fwBNHSvqAIJUzzQm0zPpzpZwt5JfaxkeIrNKJnQqeSYCcX7Y76SXSUEbUbBHt3D6bdKlQsUNc7ZM8K3DaiK36tK7xUJ56lH%2FdTEdAcBCEObQsPaPAKa%2BaY0M%2FlJ9TLuYzPmIGWap3o9hTam1HuhZ03BPO8WVbrNEkSe2uBBXZjE8LDox4%2FwFhYdShK57yUTT0BX37iNp6X%2F3GDN%2Bwg&X-Amz-Signature=27365831d9f6e2b5bad59c67a1903a3a937354ef07847ee9e64a0c8666165039&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

