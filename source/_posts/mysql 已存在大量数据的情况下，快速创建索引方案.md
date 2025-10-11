---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KORLGAU%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCICLV9sjuVDGRECIwfIrWHHsR1lfUa5B551%2BETihFkL28AiBcurPLWel4osT0AszrNJu9I%2BFZH7Uwro2tVdNOIgytDSqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7soKfVXiQ5MrL4JxKtwDazwQ0pQ%2BIoEcj9jIHV5bF1Ex5cjjTb5xbh%2F%2F2d0yX9EZuNVpcEoNt1KORrNnT9O%2BpVlTfosROAABTLeAm7coJ1Y2S5fIbrUAk3ShXziEJsJtsfUJvnbKtLd0MvC1yw6T2%2BqZ1Ry3aDFnuyLUwR4OtYlua2dpPOwS1HPr%2FA9KKcj0eOGjtxob7e3Fih4rpK4bI5YD5%2BCvFqtJA0psc0FuRvikbdDaJJ%2BCAkJGRxVopiPKj4GGjrFfO9WrZTOvIUQkrVvX9fNo4qRWpOmvD4sxYwjyHACuJ2ZQ8bidWxDeyuQTJ5%2FRml9FW%2B8J%2FHaiMQ1fpH%2FBEWVZ6XpK72B1tMwmQLW6I%2FHBHztUUMu73bY2FxJFq3%2F73h2fX8NZvY0TqXb5Kuy0PzIZ24ZC9aIJY04PeKrrJt1%2F2xG9Uz1cgXtoUEVPiCspdJIB0B4p1S82zKrx04hWxhygAVYBLHzeob36D%2F%2FG2yBRgbvGIL2mWsiZwvUsMnb6tK6ytzVJS%2FPk6gN55BEeiVZYnPRbpZ9WRSgbcZ%2FqdWOWEZwamkzSfmNDrPa7Wa%2BcxLpGDamo1C9m51aTIDLU%2B8v1ZvGisbPmud11Ra4WeWONvOpTYPUht6xwXxJpXad5WOsm3IdeUnIw3qOnxwY6pgHMvKEtkW5XzWDhoaZu5myRHJ3KWgPZSkdFkvTvcbUA1tJhaVDTM3jgD3OLRtUUjTlSZbk0q96P2IBbAZRwTlt7rfODinDLdyGwoDO1UGsDJapxkSbRtnBsdO%2BMbjV6Hlqprs5spEmdX0LhKmc2CR5w%2BLvna2bWR%2F8C1Dgvy8LPrhtW%2BP%2FKsx9uUnVKadDNolSzdu%2BDWksvBamzbP6Bnxa%2FE%2F%2FIXVds&X-Amz-Signature=abd67508dc1c65092332523503190c4276f44b65d92aaf297cb0f503f8d720df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

