---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDZESLIR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T060140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIQCvq4JyN%2FxH7obRSWD6qUqW%2BwOE5b9yTyRcZUCYa%2Bj22AIgCbZSv1RAJw5s499Ktvv4teSXqoovYfMPCvuqIm2X5vIqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPooHboPAvovkYAPASrcA%2FBTD2YQSstk%2BGRVH03I%2FhvM6SL0ztfkL5TThXR6cUDHEcsj37zg05dPwcUzsAENNshJZ6LYBqsRwYwOR%2BynGBb5hFQp1GC11iP5txsdsvehQWDekC4ZJUkIKHYK2%2BsrP%2BWlEZltWAO5e9XrSzHdhhrsmE26EuYhAHxYZVdZtfSqyg9LDFdCf3GQ5UNwaSfEPCJ2HI%2FuEwew4Yi4qAqbpaAwDJJF3GKowDigWZfCYPCwFEfWQwsSdDzdfo%2BimZslzByhZ96SkfPhO7a9268QB9eujMB5%2BRBfPIioUPJJU5%2Bz7DtZtqo%2FzV6BrluG4T3GBYt7%2F0s7uMuAyAWTRbtDjOYfjDPDnN%2BBQ13w5FXaFMVKK3ubHwI6av%2FmpVYJwp3r2meEYL4j4fo8mc0rbFSGvRMj0iZa8bfim4NGtXNgsbJCG9lTfuTizj%2Fydk8W1GAXwVBWwQc0S66PN1Cx7%2FZquXC5Uq563rzxM1ZDacBa%2FSQaZHcSbUhzWiVUmxv1C5sqBhueQTGHIQLBT5mlBjVzTync%2FU87EoammUdGBdjQosjWBy2YXr5%2FAiqx7ChHXSa1GhB%2BF6UaTYYcvsgqaGu9YvJjUHtm%2F01J0subgUAE5J5D3GoTR0WufghjInIMMILSkscGOqUBp8LyOxkWoXLk8aXC8QQXzxpmtpALslBdJ1FJxerf0wzfradD5DDT5IEDQvWC9V3Wn%2FqeR4HxMBAohlvCGpdR2BL4hSl%2FKa3gpsSRB20oUuZgfoETHBEm6UCf8D1N9JGMP%2FUkx0fcn1v7%2F8IQDHrMtO8f9C6YDnylhMWtjhOUBieKrlmMIRhFDw2qRu7FaJlaNx4NTrlbm538HM96DqPKqfM2Pk6Q&X-Amz-Signature=bcae36e132bbcccea4000fceaf90031ba3331dd6c8a64fce5a7972e0baaf2a7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

