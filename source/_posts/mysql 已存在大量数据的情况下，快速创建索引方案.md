---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQYP3RYT%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJIMEYCIQDDwR7TrvaHFll%2B9gOdZ3sUTgjVVDtllty5Zp4Qda4kbQIhAKGeGZJAWzAie3tD6Iij1lJhn8eY%2BwX%2BVVgXidc3SZr7KogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzuRHoQbblXoJB71L8q3AO0VWp1mlhV8mNLO91BnyYZnzlNTXkMcrS%2FzxnAYkWvv2oKwRmrP22wMhSp37I7Z6InQFZf9DBY5JGG%2BKKcQKjB0wsc0WHcqGE4JZOyJ%2FyUThZWlnPM%2FhNbYy629P%2FOIhp6VCb186j6ReC8DIvrMd1CHIGzzbdSCN3MU4Q1OQNXVkZjTxQcUoFaoRHVs35oGJ7TWyqbidPPV5sXmngJ89zq%2FzyWCXTWHC%2FgGqtSxMplTwmt%2BwxRmnBIKTV%2BAPScEZOiI9JrhH2nTc3uukqD8rk%2BJsVhGN1oXgh0zt5CZzHVUb6OMvOjVjox27Ij7LUCh4UCSZ3kydqRne8qG4%2Frjq7Zd8Zx4vO2o8kb3OvVKJs%2FNb6uBeJSnwfKb3HtMal0JMygMddsXiXZUjydkTuVoOry5fZQ2e54djdICkvZnCsA7Ff9Tt02cowZi%2BLOoDiG8UMjrlkdaTHvdGum1tKYH1pAvw7%2B2MzDAWtJiA9%2BFHiRrZgDnJa6L7o2ZhRsnHHzkgWKav9Yrt36nf6eUFfAqrg6lMi1OIpKv7wPdUMgIm4%2B%2FnbONfPB%2B%2FBI%2FCinRLKduMHZVVqydSC7QSoP0hMuY%2FwV2vnrII8bMk1ndpbdsOaQGcnLrYUnOWy3w8lTmTCJganGBjqkAVGTr1QtMt7Q3kplWhW9V6VAdpMx4xavceByQIQ9%2FFIIMTmQXTjRFlEHHguOHmp9mC2XQ2mKXzLlYKIxEHD%2FMHenj9VYZ9xyYoyZnMK3Pju5GPv19OLGdPNK1ZgniqmQYuFYCjt5LNXCm00ulob3C04x4Z9i7C3CtXO2MFooTPODOf01SWA%2FGqdzncSIQABY7MKAD6yg2Xwj%2B4FG9HlNSl1prIDM&X-Amz-Signature=45c3a00d1579773493227a50d803da05b427dbe12c528915f1396ab52f2a16e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

