---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6C4SVRC%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDL509gmZMN%2FfrVWLyjz7xoHcuxf0yII0s1wPGDhfb%2FAgIgFCozIdRR4wtmoXH3Xr8pE43Bi6ehCAas0V4j4%2FVgFz0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDCeTsjOyKoXGtFta%2FCrcAxwzfZKvVelxVj%2FZssLHlxGT5%2FaTikCt6mAYcSzZXB7lbA18GV4zip9x4OgRaaY7BxO76jhV6y3dS7Iy9IlE47D9vLqxcXm5F0O8jmcphDi0TgBI6vJu%2F1mpyekA4ONX0kl34oKjQNXGw2l1dJi5AiRjPqUBvEbAcIqs4MLxy9Z0xaacbN%2FQ2SX%2FIvgdTbN91cL%2BsMPMQIMPJ9A%2B%2Fj6F%2FXdpQpTI%2BF%2FSFvh04DUAv2fzVteHy%2Ba%2BCOAqizwrZkTzLtvPSL8lluGtZFjohT21KssUOyEfb8VFL8V0eZ47DWi27cYL%2BY0GhVLbIMuR4IdHo4NHm71NrvfOlwoWWGRtLFcCtVtwiFOwEZoBCHqXWlB%2F%2FbohHdZXGkM6C714hrMPIFK5Lt6pz863DlmNBc8NTCBods4hCmpapUjx78DIXCPq7UPM8T57HFhn8tH9uiYg5d4njASoQ9TpHMxogfeAf3lSrH0AR0eGt0pcJG%2FmQD2DetYyXt%2BHn5ax1AUBqxDTvxhsEgC9Gp7J%2BZc62SK8zVRR2%2BPaU6aB%2F2owlMmT5jqRVu%2BOzEzyPB9p4Rlp19uAzeY25wr3pOuf2zM9Jo0N%2BAZFwKIqJ%2FylIXQ1xTRn43R%2BWIQ3yMYDOtzn0D26MM%2BwwsYGOqUBsKoJwfy0vayF2r2DLlToK7OmUxMLad65waETs2CGr6LmZu5TEEruDE0aR2eS88ZugQhsLtq4rqnVle8kl%2F4uSoYE7daCYlVkLcjZ2xU9F8e%2FdY7QPHBvRogDxR79pjIGi3o3LpNcoeLoR56wuvE%2BhGJWEveV5l8Kmn0wh8K343KeIIx6eq8uwymHHzW5M2JomOQ3%2BAT7eypy7wBdblCk78KgCpYb&X-Amz-Signature=7944c41af15ee827822fb2c5fb6d6538cbf0fa0a5692437d7b62a8860fc0fed5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

