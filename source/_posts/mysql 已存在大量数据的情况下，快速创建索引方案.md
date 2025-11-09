---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7RGK5U3%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIGa4xc%2FXTc%2Fics3fWgmMzUK6BrKYKldqSy1%2BoQQiy%2BdHAiEA8uFYwkT8O3Dm0m1x5loyMibQCRLeYndnj16AYLCkoTcqiAQI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCSHKHTa0CpzPV9e0ircA8UwAk47KwXx21mWWE4xTP5kVZ%2BdMGnAZnO1%2F2GF1eACa%2FJTEPxLxBI5ovF3XH1PER3ky5RJ68ZHVhtjNki%2F7wUgBa%2BAE%2BFjiIvcDYNxxsheETxTx3D8%2FM1E%2BJ9iyvARpM6yznF2ZPeA8lbmv%2FJzmqeWsVxq1lx202%2Be2FpCobjfnn%2BYR8YDb7III7XmGqU9SZrbESz3JQzzJC9mViXhKjpYywiugSOPlsUWJFS73Ge0OQhWdHd5%2BnyGmJVUBl9ed9ubryZlGmAnrHtp9sBKDDx%2BiE33gZwkkAHpHBGLZADvfAX%2FVJpE8FRj1rmpzd2gLQVQavtYTAZ0Avu4krvNmSYGndXYmkKVaoMHTeKycYXsAhKZ5MR%2BaGPS8nI6FnzKciFaiGukqJP0abYGLaflO%2BQW4olW9hqTA2ZZiobtP5B2gafWNrHtLvHDHieQFJp5V1UNgFFu0uxE%2F6XwDJEb0EpqBjAyAZVSD3mm8fBnoQZ0UXXHKd0ltptJethXk5P7%2FbPl3CsUpCibbcPQcUXuHWm87Cfu6u0e6W%2BDUjnxhbgEB%2BAK2dduHQCIseLNwTm1G%2FAMoF1ytsBYmGM6Ap5H0XyieAQE3AK3k0gs9z0noBZjsMYy%2BgFgTuakDLHfMLCxwsgGOqUBsD0X6%2FTagK6C6RKWsmmowYuSQN552MwaJ9o2U0U9S0aglWBnYbTA5juKpGMRrSdY1oBZaMBfM%2B0JAvIKg4%2FNj6%2FO44Gx310Dj5BTrYumK%2FQ40idowoIswQM%2B4ZtciJX5DxsnGsLnjfu4%2FoDu79AOICV8bHTZivf3oL0QlFuAw5Alxe5lvJa%2BY4DQL09RwgM5OjTDvjitPPh%2F0%2BE0XBQ%2Bvb7ObaRJ&X-Amz-Signature=9c8201547b372c365f0a161362f6fd32c39d6b8f75240c6f960c0f815d8af7bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

