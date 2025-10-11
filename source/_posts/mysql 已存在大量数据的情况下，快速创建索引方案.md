---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMSQWIDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDtftWTgUGJA8PY57CVOscx%2FTPeMv7u2smX74hlywN%2BrQIgPmVjyIGtTI4n%2BCP%2FQ%2BZI200zOHp7lmmLkWZl6986sksq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE8U%2FTX8zJFZTNjHeSrcA1SmpUzzAAz76hUTQ1wLw4YLWus1pe%2FLDHuJ70U%2BhCnFswzRsmi4lD2sj9X8PY1uob0Cp289yYY2gIp1C2PV67G%2FACQza%2FlPYY1zn9pLMcp%2FU5sy6hyMZ6F5UU4rateD%2FXKsM8VEXmfL%2B8wk4zobioSkOo6wJ%2F18TEUImvLRs5fjIv3N43iT4aOJ2jREnEVuk9qbZ7z2CWJZye37xJ64wY%2FfEZ6eu5tCv7SmiwPxdGzRPyWrcrtRzWwC7XEHNnZym4JxLQTzBB9ua4MAIJomiWv9XBWzuHwkOr0l8ECEEEjzFGH9dRAf5BcIIkC7RmUGdcaa%2FCbt91jGNrRFIngMJ5AWeFd5cobqWsA2cbxxN3Z3CLePcVRj%2BrKYiKA7veJERnbDLgLViVXDIp6xK5yG%2B2AxnUef9j4VsIdNUCFAHO%2BXldlinvVZnkzO1uXIhgXgSdB13L4l3BojHm2Lr7XzgcFy4RfAqnFrki7%2FmVXnNHnDBsaKWxQJZUCxm8TkOsuPaKSlTTZ3qQaw16YPhv1ZicdQyViRiFsRuc1xIx1gHQ2G3tgRR5TGkR0sfo1ge%2BrfQ8Cf4mduGMKy9%2BimmpzD%2BdkGWuOkqx68Z0Yp0aMMJni2PJHzr0AhwCyyQ%2F8XMNGkqccGOqUBtprjTd5zks%2FRsQhxDduAUWKSHIDg%2F%2FUm0SZH5VqcdAhABY9rGgg06H5bgx0dyHWBJbCYv4sFml5fpqmJWEUmcsCDPlTfMmyRLk%2FkoHRSl612e%2BMi2u6YEgdOkGrFhoZh5F2QGvJouUntsyzXpoqm%2FBq6%2FHjLoTLl0Ph02IjPRteprR3vonyJuZ94WebYbJwJ%2BTmdep1orQgju%2BHsmECMATjyELeF&X-Amz-Signature=d2c0e8e2cdbe032760e0e7499068248a4ed612db1b2a8efbcb898be197460c02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

