---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CGFNKIE%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T190036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDq6dW0gDRvhtrfu416TJ4Nk56wKJUg0kNtyv90lISssAiA9Q07CYZFd7aj0cGV14mH4RXUBlHzASOG8fQ4uoDJIKSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMksGr2J3i08ljwK%2BEKtwDap6sxbvyl0Dinj57NrCvO6c8dzyav5U5Iz3Mfdc4BKiTFyWn6SVs%2BC4FPJqPAE5C4ayLzwVrb5SJJXcas052kuCeEnFZ2kAj2drI8VFWXBUADL3j0KOgFMIavxTz%2BWU4mcdIVt0vji%2FC0QP6rSoMZj1YgNTXbHGr4u8OgwlBwKnSk2j6wEiovPD0uw0gpLnKr24nBPOn8nJFffb1LbsLwniPdRmET6Jgz%2BiJRRJyaXEQ5YURon0z2qB5ufrJYqmYL7JPq4uq6oiSe751lKxHX9h9XLHON0NqAR9%2FYVt7Ok9hi4yqWx26DoM1khibnVdDiTxUVupUDpzt%2BqQ3RVIDjFVo3GtAcw9%2FWEbSKqRmUrFYQ%2Fz%2Bu1hLDXegiXwtoFgj2DBmPmvjAbypfYyCGvqzh2GwWAYBdUoTRzpKAeKNjFZKLRSovrmvAmgsO39NU1Bfy9lGAFA1bN7LKMG0b4nfaJh9dcrC2cnF2iQwVxnffvOcPDpdemwpuGDLRnSV3gumxCa34iVZpE0kYEB4xV1fEMoUq4jG%2FSXpIEVBHqlO5yZmwU9M2SZlCiYLjODCVZl31QwNFnuSi8zN6c7mHhlLmuz2oeQCYMR5WcXZUKHdPuyT4kUi%2BBeQpDEPiVcwpIidyQY6pgHk7dgr5yO0pDRjGLj18PjQkXS8Gi5B6qlPzb2d2z57yFDc32AueJZ7KIM6N5086O4AW4VKdzjORtMW1lyDQnUCxf9VhQlEi04uX%2FxmgarZoSEkpeB7GqxBKf51RW0u0eNUMwqKZ2DN9WlOeqTswnqoF%2Fq0mKslCVEwikNtWGVDvwJCPZMjuJ%2B4hvyp9SppDhNiz%2BmcI98794XAsfHksU4oy2QFCNZZ&X-Amz-Signature=98980c8f21cc774046378877dcf368cc57c4c06142793b3f2723caa852f13239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

