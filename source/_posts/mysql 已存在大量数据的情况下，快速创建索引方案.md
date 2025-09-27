---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664XGW6AG%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCqEwnpU5XeLgXZBvFkEo9VI9P%2Bw%2FTZKZlb7hM7mrYEDQIgSCPyn%2Fayex77r8VLPvmWrhygxgoic788eHg6pNFgt4wqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKDzTivQ2BEPu%2F9bCrcAys2AiU8zCK3Yg4wK%2FOgYW9ssaK%2BtiKd9R2mXlWhMgGiMUuEjgFveAL4NmKlbNHxZ1BWN9gU%2B3HZ%2BWB9Q%2FU9Rfi75WiqUhMdF2hkOvz0NOf4ssAslx5jS87K2BAC6IUdDZXM1yuM9MYZr%2B64X6Q64bVwmQP7N57L6YX9v3qEABd7JJe7WrHCJKEm5fbqxlnlhPNQmS1sookqQkISWoUvHce91GIi2xxGVKmGlrBd%2FlxHxGiiO2h6LyCkAk%2FjZim8PuvZxjSPSh79r9FAuXWlqRYms94YEZoc1Lm8OUSjVzJwpvllusp%2BerqVQln4RLuGi5CVYyPn49nN3jVdNlNuC5YfTsEmJ4BjnvwT7qt6uOeVbiav1W9Eu6IUOEwx5qwzMqwg0zSNwYulTGu2vaj60hAerGW42NXK8DzvYq7SdY%2BiniKR2%2FXCQhy20VfX0CzrVeir5%2BiySdNZX6TFzA5mDmTgYZSN4C%2B09LeGkpToS2eSdlCohxlguRDIXmCEDZUh3sj%2BQ%2FoH%2FfO2Cw7BKP%2FjEDA%2BCMwyYVTXKkvkKJceA3ryE1gpstsbem8rr2vN1uEoaXYciJbGbKLaG2VgVNVYec8RmlKCAWvVtMmhzGjwQtVREco94kJ3IRR7jtCNMJDj3sYGOqUBBF%2FiOo4P8ImXIYDWBwEax9k9u8lKIfL02jwToxwKvi8onBBPGIg%2F%2B0mr5CvHahskjeWpdx0TTKIFxfie7cgbQ%2Bx7VEeUPOKRyCMA87zaZ2ifAgQsq7aCRY%2BFjLsRzDM%2FU9vwiXzIfVIechgBto6Vm9Zo2jLkieAddqoMpN6R0dR1oij59jC4p%2B5aVaTK7%2FNWovjWUnzj4ovxnwOswjkkcX2d%2FrsR&X-Amz-Signature=4b160e8cf5a1085bf22e2c5fd098b32cb07855155ba8b7fadca2ad256965c719&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

