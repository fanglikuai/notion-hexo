---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2POHWOQ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIAN6aLLcUDHJen%2BzNR%2FyH0YLg4E67u3%2Bo2PCVYGlI6g0AiEAyF7n9gVd3nBqLGnK0BY8aqEBW6oSwfSBjKbdLaAehRAqiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPolKqls0p0zQcoA1CrcA9EjWlDmEZIWgZwgjodwW6y4LPypZeioZPX0OGoeUk3pDEWS6gvtuUDIQhPwns%2B9qjJcYA9W9XVDJTvU1WIY0NnZI0fGrYm0xl07TM4cFM35l%2Bv4RmcLoqOifTw%2BKMGgUYX4ABqtdG8sxJEtTe3DPhaqKXW48hsNbQ2PKOz6w%2Bia8dY4kkZei7JMdsiBjofgyLTxu0YnYmP33uRDZD8dy5oPd4I2pwE31Neob%2BmU6mljW%2BiXrXk6ZfgarQF%2FiFmdCWwi2UHsE%2BtUqI%2FeoGxNvd%2FBgDaYGFm%2FAbhCiO5fo7SPs%2FvQnx%2FAGLLkidIohRw0fcvIxYr2RqIdMif4FFITUnCBWXq0aHgmFKwMKc7%2FHuPhkWBUggd6%2BDQLUiYJbP1%2BRV4deQL0KXsDYtUZ0hHPQTkV6IAH%2Fm%2FOmh4nSeCoxZwo2RynyNKToU3%2BUZOEQSFgyEYmA8W1SObIfteuKPkH7x%2ByrV0FAAIrB3mUOm%2F5n6%2ByvVJvzx9DIr0beoig6FZpv6YfjDpUGebzoe8LU%2FN2XmSYwHe9nb0C1xtjKuhZFdS7%2BAq%2FitUJBzF%2F%2F1m2c%2F3Yy5cWxym23TGUA33WUkucSXgYKSO0bjYsbWG2CEpBwMenfWPG4SDibALHooKlMOynz8cGOqUB6qf4MvpYx%2B8eBjG5rCRkOHqVd0DgyG1Y6sOgiNoYRBZSPNimvrflxJparlSYrzEaaMM9bf43p%2B92yUTVKhTUxFWJiXoKA%2BOEB1%2FrHAcReqmzVLjwvoF%2F6pRFjpRJRQaNUjrS%2BqsMNmmd%2BHvfag9RK9d1EEjfZ20SoOk8YQw1fXhrB1tcroS3eYtW6nWQB%2BC5O5VqmO4829MjFgmEbIfQsNRyMcYM&X-Amz-Signature=d609147602e4c448c1b432c13fcaddd750ff28f71b3fb66568f397520c2bc012&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

