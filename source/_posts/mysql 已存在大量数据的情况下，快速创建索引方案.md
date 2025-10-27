---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UESUJ42O%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKZide%2FnRARIJz4MVmD5AKyyMdahIgIcE4GIzOg%2FiJrwIgLDGHdn0uXuu1gKQh4HBkvHvxQhfz2iRB8hRTdd9mXvkqiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOsy3hXQEzJY6F7myrcA8hk1rgJh%2FYX7nqD8aSa%2Fu4rrzOIPZxkjF%2BEHhH9P3zjBhv7%2B0eCwI4F56T%2BkhWX%2FGb9tyGpfXuioPOGPA4MOhXgNMeSfbYtYv2TJn724EmNOGP72ZMQoQqkv0UzhoE7tb%2B%2F1aug30cgLY5RxWOW%2FwxvAcNg3nO4VrraQB9DH2fvX%2FIp76LrVQmI7nOtZCV%2F7TIhlqXpdHpG3AbvhfYhrBZEmmwrs1KMRnYnxvJYyzpNVzvsVMhsOHHzd5XkXl2tGNRo9MrXz8WMc8%2BFTFO3Xg0ymi6E5ehahoyzXSDRBvBfzQbFN9x%2FfNe7FZS89RbI3JbQJ4254fXY3cKXspWtiVdTVA4BsKEH5XsoJgY%2BpkUkX%2Fzb62Zy6bUc3FysFWzogpommoruuDUyP7BKHcwq4NsVXoVyKBaTFhgfNrFlbwebYv9CZexmxa1KZ6kyGE8y9OLf9mhGqaCERlzlSK6kD7f2fHiIoMFoxppOM0Y%2B%2BjOva%2BsANdc2iOkQhk1ECK25e7Al0KVxUC%2FAJxq6sKLNx%2BRVotc23OE%2FFqcOTAs7UPyZdLaEnRTX5w5Uzl7UH84MvNKFg6ruMCNFr4%2B0rHqFI2NoqhS6VJoW%2Fs41TnRo7F14165%2FvlKn2g0zq8O2MLGi%2FscGOqUBlsl8zKPzFZcuDwJy1Po82CyDFFi0Qu8fN2pzVCBIdJ87Gluvk32XYZeEAnCT%2Fnfd%2FSV%2F2VxONMn3JKcj9V2rQqkrT9H0%2FjmS0Oi5BC3ffj6gnTf1SDDVn%2F9p3sGujjDSjlgPSHixyBlPjUlLWKjJwqDBr0Fk1%2FDMr4j7E%2BNRx0hr9rbLV6%2B0xnkYatwnEFk4p1Lzxfsn4S%2B9klXjv1%2B6HVF7a6yK&X-Amz-Signature=74dcfc574f14ade252b74dc011e06dfd1becd1437d0f23406cdd26fc67e4eb77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

