---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7F6J5M7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCsi6MSmeeFLde0xIA6JvtPBG7PNI6K8r3f14kzsutEHwIgRejugzFGrfvTBWkb5tBwJ%2BDky%2FyBf1mwNIddIUutCMwq%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDMkPi7zLzf4Mx0b4KCrcAxPry%2FAf27DtWHk6KuuErOyVOHPDqnRU7dAWT1mjgIg11NehaBkp0bqb6jwfYmZUvwDwEb8gvME4Kz1BTXiqK0ISQbys%2FjsYiNyrY4qExZ5Wri4kY%2F5W6hnyvpSCesy9msYhPSv6100xM17HJI3rAvVXr8SkaH%2BzrFLUR%2F76TdDII8M5zzMHk1hMkEEiP1WsCN%2BESftagTQHzg0LLmbjsk%2Fs2WhaGZiOTgXrgdSEPgUJWmSptcOkbJ77X6JF140WgyrszQ6Y%2BCZiPSv8xr36ojvLAAycQxZWoGuHY6jzUZy%2BLTAXZHNCsWKdJOtgwpx5dEFIXXT6Rxdwt68BfWrhw%2BmEIeOIpEa0kbRFAbJx%2Fyl%2F0uY2aM2Bw2I04tf4fOEFTPZOt0CkALofL%2B6OFnw0O%2ByjFIOvFAhChI0Ra%2Ft32G%2FhtuwUCmlWoKN32vDEoPb600uo16gRUnbs3wIGi3gQYSgBCRwEaWaNe2eF2dz19l27hfjwa%2BmAW04dFkwbNM9s01NizQH%2Ff7q9ulBA9JKTNi18z7DYzXpWlWctLz7mzmawbqe4AO5XIOIuEs5cz3VE5aja4UgMq1wRmb8E9XDvhbsibkCrIt1JzYFxwUY1zFZvb9LIh4Jbzd8mE3iwMJj3gMkGOqUBPwAYPxtXsgyMGuQtWNnr4KxMxfpXmxCCry4yw80Xhrq5GOxohjTStT6YJS%2B8ePLvJw5vI7BBD0WrfuOjK0qf6%2FQSuhK8U7iV%2FWetmdLNJPT2WmYiO0ZR11lEw%2FvhXheI1M14nmKirNJ0so7MH5kgx3qCUc%2FMy6ZgtKmE9sMCu9bnbyjPS1OxEJwP1EyBJqZ0sub3d5Sndw1oH1z3ef8EnVrkZMtp&X-Amz-Signature=f98fe51b231f188757fccb50c96f4fc826b7b263b46218a3ffaaef22296caf00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

