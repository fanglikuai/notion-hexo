---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWL3KSRF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBuxQJFGKoyO8mIpILXY12zCHsdWcTwkIPcDIl5xoQ0AiAmF2yLOPu6uAF69cJlNG6IoLyjhwKkwDnAA9FiSEoSciqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKcwHhlRnlLAxTxYPKtwDZ1lUP%2BmUvRCr75FTLZDKLAehmRrFM6SByBrxWp00AVu79yWtuLO3Gu3Eli63G1nWAGUBWW0zBMzmAsjwgdEAjWH5Iy7LIhm11JXpKvMAxbNcC9U1Evdf1BXVPYHetLyF2RfJCLO1HOFAb4jIcoQrIsWicJhzQ58wfhVNOSZt59%2FhXrye9jVkp69gg7E0b9ecsAG%2FoElUdbkU5QGCNIXAzTDWUreexAvy1QvS2%2FbQx2bNlw647ob%2FnWGZEhuouITbIXIDfaZzRpomWlU9Wnl2S20WWYfEn32tVL30%2FIi4dk0kdMgzG6mxofCJlvSvadJvoMp0A7Kf0Hyz1wZqFajNJrRMjsp0A56q27y3fBN71uW6S2IllxPrZQf9zpfLgSfBZ454BG8FaF3zOvbivnZ%2FBZ5DD22Nbi4aSiV8cP7VLFlaVgKNwAsL6mJqTgx2c5hV%2BAh5ZEkHxyg4sTy4S04DLTtTJWS2D%2BQ1j4ld0aLg8N%2B%2FXKeLva6sTGCAzPSOlE9oku7JAoGdw0X2IRGCOuNDYe2xPYfhqm7NLJNGGB2VMNNOCMLutHjnfUvLR%2F23EM0Ec0EQPqFi%2BOxRoZRTE0qNlIf5l%2BydrTslcFKd%2BX3uXnHKURJFB1aMai4x0www%2FOGLxwY6pgFLWkB16aaY7C428bmKp1WEBJpZDUuV8wN2HEgpkKMmnf5erRb4lkKVtuIZrusT96riE66HovlRAzsl1wa%2BlZmlzqV9s%2BTG02%2FqWHUsYOILQych93HOaPD88Nvx919oJiGwohJcbBm7Wx5iMq%2B%2BjUfpM1cX1GkfwdLURXEY6wxFFnrcAJFSe4HXI50jLTaLERgP7etOIYqJbu23wrF99lG5rBfL%2Bzc%2B&X-Amz-Signature=ae91afc392ba5233648b98729120be20cdf8798691171cfe41519dbb4383ecdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

