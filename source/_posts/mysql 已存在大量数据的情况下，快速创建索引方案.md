---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRIRIOK2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T140058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlRayQNWihGHJkelA%2Fu6tSDNFhTsakkzY7faCZOIwd6AiEAgfGqIMilknSq35Fl7XjXecHg1t1wxqY4hawZ6AYUDNkqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuauyf2y9UCNAWPeyrcAwSS6mj4RB9dkox6jmybUb5VgWmZmlrucuPFcwwkvK3HF1jxShNSQqmesHFInuQ0b0o0KW5P0w68LU708l8uIvapqMRWOiXLyA4f5F5NPTwdVqg6YvwQYoEi5NzVvqgWiWQXcSQiOIjawOvFY%2BkllcDB0qsZUxUHRF%2Fs2ccrdiOSEZu1qoYdoNTxpxQXeq9C7n1Shv5HFFJ6K%2Fj4abrZpO19sggAahXWQ21uQ5pdbBV%2F6yOyuQU7eYFSmWooYPhyyJWOXZukFqv90Wyt%2Bg5PaopJpiSKmQ14WtmJMw%2BM75SE3fr9SAdYYn7CMGo54JQh8HFUf7sr6DQDIz%2FcxWdz3TrRZzPdususqT2afQ82LwHaJZKnEFwRs2UDQZRoOiCXTUqs%2B8TMYNQy45OAeEHx4ThRlXs4t496kSdZWn3uvMNcKO18s%2FeIf52vYNmYFiENCy6g5iM05bqTANWZV%2F2h8GSvaA10gjKdofjt766teHnnezJlILvpjXDIVKbomz0dqyyX5oJaxwwPk1%2BeW5k0O0abIKGsf%2BKK1n5g6GUtikLfRvhb3BIvGYrxrlzwNtLiFaTHnWRQ3suQ%2Faxk3wGfN%2Bd8%2F9b4pnEs%2B%2BTMeOwWWkG0Lb7BiDykPLFSUtPvMOqTrcgGOqUB4ZQkytYTLEiPtE7SarXUti%2BFFj8Ti8YbN0Bsbzie201h1sFgEuJgTIGQsaZr49GMW6ZOkyOO39lGhs0bmKf59UakryghW3YcKHKBaW078QeiD3%2FBUidfFaEwWk6IV5kXlQS%2BwCGrbjFRdzXvCd9JCkx5bgPMNrQ3VchxrgRkQSRpoac1K5NnXRJVuZabSMPNreo3XwrKJezO078sGXGvZ%2BK%2FBZhG&X-Amz-Signature=b042c81b5a920a8e8c5fb40284918caf3794d260cfce54d7072ea99b572eaad7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

