---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466346PR3IF%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW7tiKmLbnhUs93snAz8%2FQpP3E3sao8fEnuo09vzWB1AIhAINDWZtbuC371hHifXKtKYbKbqhOSbzqhyFzHOz69n6%2BKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx1h1hpU4rx0mIQ36Yq3AO%2BvftAJhpqnAhr9TiV%2FTEBP7MuaY64ANn8PWnavZ4nxEs7pQ3KALZAmJTIN9UoSw7m1V1QUc%2FLZ3e1qnwo6GEkmkKSGAmoX2A3SA%2B4hrSytrW4dovUnsSjmpN1%2FxfIHItRQdBO8NtsM0Nl%2Brb5gBuedyrsvkCMcp4GzSX5jMGD8O1aX%2FhfzJJiVVTlnNc%2F0GFjI8nBAf1GXbRTXygHA1c34SYjYlmOWBq00GeUbPpdXHnN4kQ0jH3gEFFd1Fdu6wfAj9yLfvhv%2FJFhu%2FrTv4KEe9tC6QzXyjBHRQ2XlofZXAeimhU%2FgQ4W4DVYELgoj8NPa82inck1VzBfJKQ%2BD9EPLxvnLcbTt%2FkHEg0INlGCOvYIPCKgTSFBmEblrBFUbms%2BsvlH97tdTcDjJNPQ5Qmu4WrAMYe94JEpFZh05AY9FalPBKHaCNpO1RrSIoICNxtVBKJ%2FNMScTZXurynFXi7BcVoJPdMv%2BVbtOlrfYgpL2njk5KFHZm25AnLJogGCZ3OSgEWt0N%2BYPiw8m3MwaFXzGcQ49b%2FJY4o0F0YwBqnXRk0S%2BAL0KLJ3FpbE0ADzmIXWD1ie0xAPBrz1x4riMqTt9ti%2BD08LAlPtcwYCoFC8ybLuEug45zdEri87EDC7mP%2FHBjqkAc4UgIQLVKWpdVcaUK0M23SqM3TkLKNCpiXODI1v1Ljqvq97zOKF%2FUki8QYu8d7qd0gJEsi1%2F7poOvGpCYT6LJJo8UIdxq6XEWRMHXSzZT0Nh2qjKMwIbOD8NoIN3ZZpFjl7gdjVNp21opaYBaNp1n0Q%2FE3bb7erMgnY1ECXPHK6Za4LdV7F5NdWLlvduX7ADf0p8Jq%2BqRHXrlOrHclX5IYMGzKe&X-Amz-Signature=d30d5e7394efadc981ed5eedc06fae2c2ecdf7f829221bc1dcb1757604ed5272&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

