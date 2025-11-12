---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKTR7F6W%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJIMEYCIQCK1F8i7WPteYiDI6H%2FaIzYN58lf4DIQHFueLv62hTQRAIhAM7IyAFSsMxezgnsUQLEpltc%2Bwlhb49l4M%2BPJU%2Bnf2SYKv8DCDkQABoMNjM3NDIzMTgzODA1IgxG6ASkocpbXHCM8o4q3AMsVX8JuZxuh6C%2FxkL957ONwbfplOsC4mNfxNO3e0f9NyCGoFwp5%2B8J33Je%2BeliRELLw3hmH8XVq18YcMe3VowOMh9rYbGzK6CNjov5UK%2FcJxk62cj4%2F6HgJ1FAdaIwWeVB0qT5slphjKhy68ev1J2XGhxl6bTDbBovewdRP7yuPikGTEaZMbQ3IHM98K6%2FObOFwyyGef5jyet%2FBiDqddswd36W0k38UQ5wUKYU0lu1INAS61drSaKBmvPe2T7tTPamvZx07AvYrWHrnNhq%2FNftCtWMM3UzXCcS8UoTre9OWrftFgi%2BgPbXOSUd0dS4wR6VnVZomC%2FMC0pvWkZ7GOZTOg79YO6SsmHmqJhZLIBOApke32C9vvrun%2BYZ71gl2eCLCLGwvJBYYcdeO0ggaAystd62liTQoCrTPWrPiVsT2sfY2BL5jYQb2enqP4v6x5BIDq7RIQlf4BOTOL6h40ZDi0BDBMwgUla9Jbzm7JBNzabv4Ys6IGwtWdIuF13OGQud9lIKk8UnFaSkaKg3ekEMwrGt9e2ZRiTlfYz3FLf2OhgimoY9ygGb7Jr96emgBwd9sjCuS8TMJYhLsLXLxmvcJLDoUW3foVwH4ilUjFRLoMIJi3xVqkl6UOPKPDCc3NLIBjqkAZ2yGNc17XUo%2F%2FNLRNLQ503leI3S%2Bao1bWKd7a9FEWgyVWO4F6rNj6QxLK2vwdjZ2KbeL5RiVjDprRXV3AOxM61hKoV9xOY9hzKgxAhl3vAo2GyPWoF2EZXyuG9B3%2BFF4QP5s%2FFSW7iYLqc%2FTlr0ab06H7EF7GUZENf%2FN5t0wW%2BpOWWNVAqtgGQfCfFaeMQ8BHDqnY0iwE8CynySrtYxsZgegv4V&X-Amz-Signature=88fbe2d998ac1559625443a87c1ad46ea0fd276d543cef7e4909dc2f59fc3948&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

