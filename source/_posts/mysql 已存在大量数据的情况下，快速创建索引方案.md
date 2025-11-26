---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HIIG4IZ%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjPqbcfmdLa3nYF6N43%2BEkxhQJPHfyTPOY7pwfxSpHDwIhAJWCvuL6QFXT3bWz%2FEV%2BulT4xgg7JGYc%2BQ3P8Nh76a7BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyu%2FaGd88b%2BgL8RoeMq3AOaHAHzR6jGwjZ1Sj1Xo2g0Sg3lIJb9pF1pRUCTTNdWyoC155HQ6ghhirrojqLMSuEblzl89MhYuKha7wQr65KJ%2BmEq1dOTa13HADadHBZ48j6kWn2ZSkBRXSQhF4P2HEFQyGLv3dNs5o4zO14uEIgzEGG6fEMWqb9fisfE47LN9QS1e85RBCTJwBMVEpBEfH4fh8mBqivYEw2%2BkDID9Zq%2FaclV%2Bx9AZ%2Fxln5UA%2BI9ZdqaQ7LLwx4oziHiqrgylK%2BkE5sPFV3kLEZ0vwYfIxDPJUSpXHzpLPM16lFjr2I8qdTDBmjAMD%2F%2FzgY3BwO3P2Sl6LS2jX%2FQyJyGC7KpmVM2rKt4oZe8YhdkH0cx%2BJ3FGmCXTJNTqnUnhbWT%2FzJWFg1O3eferPjFCFRz6LGzsNwAVnryR30LdIhswyHvH56ThGsguO3nO%2Bj81Wfae7iYGZ5xN1cduzz3Ox4D6nMU5eoxeX9QnEWmNxkE7OQzKhkec8siqd4b1Y1giNyIqRM%2FYwxUi%2FDNFVeqH%2FQZXyrdpnYJBvtZ0tOEk4O8J4InFjFYCIf28CVK3yuBONUpsDO27IqOFBzJZbuegUnffVyySG7G218cUhbqo5nMfre%2BAlawle2zg9gFbo5NGTsNDLTD17ZvJBjqkARwyfEHiN8SZLt4h%2BT%2Bzs9F0X%2Bmt5w2cP3fPkuA8TcRanDZrOw%2F1rdLiCIlmtwSAGxDEZuCYGe00c8%2F5mz5qQBt4efGEl2Nk7ldKLyj3bXGZf4MlEy0xWDhfrY858TBashk2sR1802GIpC1tmtTPxI1eSWwvOgxpbIhgsr5j0%2ByxG5pYoE7%2BG0%2BzdAskSFtwPrd%2BjNVCu8J2D%2Fbbx%2BICPUQeZyPB&X-Amz-Signature=e8840422dfd410d1b1aa3981f02715225888dbd10f5924d91901a20a0215dda3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

