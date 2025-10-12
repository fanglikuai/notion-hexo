---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OYLRZZ2%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T110058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH%2F1TAQL59HzyGUYMRUlheqHEkCfM5SCYImBbhQd2KP6AiEA%2FaQtyoApy479SN0sjJcVH8kBOaxGAcD1%2BtKcwZqOPSIq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA4QwTdZybk7A12aVyrcA3TVeWUYa1OBDb%2FHb3y8c9DGqmOC8f0i3zisQDBZz%2FQ4dhBGd3Phmi%2ByLJXIAZBcfBQQxt0TopLs%2FH2Jvw%2Fnb6pO9HotKXjczTPFQuYFghEKjSsFVpN5xVlCrvWsTb%2Bu4dmVJMvKAWHqVyT8XPYoKWnqb2D%2Fc69ZEw3z7ffDBqpb8zP8xj5ynbUbNb25zb86UT4jHzswAOVlmKgyfQ0LTML5xEA3c3vVXVpR2E8HmNA8s92vqnxoqU%2BKB4J9e7%2BunQ3bGGjnXy3eZamp5SpDp%2BX%2FsRREpMEaEziMnLc%2BV%2BDLFxatbMIjR17gwtG%2BK1k9XgNlN%2FxpYGy3%2BFfCMl4qTBsQPzvKgdc8S1yyBw2w07sHYoLKCLny16mIDTUO2yJ8vL1sJykRJ3X6cS1kJ64C5zoHMitg4KzzZCjcJ2UlOqHvhaUD1NpHcFLWnBzqxvkCO6KbWdXa%2Bkg1dZUSEgiUWqR%2FnZ0wgZia%2FLdnAGDPZMXSdn4HTRJcuTii2jpFvG3IABhOeJwVEoVpqWxdqiNYV6Nn%2F3c1ojlagg2p%2BLBGR2tCnPdVEQ6tlsATqkhcVCO9eibTgGts2hExztnmQslkJXR5Zr6zg9FZnqm0pBInVQ5%2B%2By%2F1zOBXjW2kdrTKMI%2FsrccGOqUBkCMbpyshdUEXhsxH6OOngsJP0u97774q8eFCSBPvrNTBmVGYdGJ%2FZmihWXOBi4tVPMunDADDodBJLTTxIK6CLv2GsvXIE3VCA0dnOM%2BtalyGhlIkVUMl7%2BvFWeaPNLgVoR9P1fPSOyd2H9uR33kmZDr51lfiKrxIm%2F%2F8znsbG2lPWd4mlaX4O2eMnPvInbBjPriHSYL3MhKYCNQancKwIKcqqFiq&X-Amz-Signature=184ead62c4a213f2de9051a160cccc7e15bcfb5300d188333afb59c20a923f54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

