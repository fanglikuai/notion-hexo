---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPJLOQQM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEmyRUKDV3N5ailaaQw7N6gqo6AplnHKSgZRSep63%2FTvAiEA30R3rUr4cU6Tm6WqXq1NE1AwYswlrz%2BpY%2BfZhoSqqB0q%2FwMILxAAGgw2Mzc0MjMxODM4MDUiDN5MENrXFX%2FcQgjSXircA8H%2BogMw7Jf91b7%2Bph5SOGdxNSXRKmNMHEW66HDo3zJe4vfJY9EhL4W6xjJ0NsukPtH5x9Gpc1JhqXIeWec3fL6odhk37MqsAQPun99O8IQFfa7RqxeZ99irRqKbn1YhRVDUaoS2csOSY42qea%2BhFAnPCH%2BiEvpxQcIWVU%2FNLMsGIY%2BmaRllBG5bBnxWJUl0cZtjPiVR7rxi3yzFYayPuu34CGJfeQF1tt%2BqJ7E3HGtEj6vKMcSyxFuZfphhfaoW0%2BgS7IEcA9Y4YYgf6zTDOOW8ftWhAwYmHUp4EUte6HkPwd6MVFrXmpIov%2Bu5pcuEyl79V75gruz5cjwBmB9QBMEHNCjMYBQtZv1Qk3EFf2Pnj3Y49cD1Xr6Ts9zlcSBWvbl8khMJ1YdGHN2z2cc%2FicVkMglnOlDG9MyqVlcvJrLLecqcgBZMsDuCqqD9PZ%2FhSfu6RmP3uRGgVUcO9Y%2FMvEAfR0pqlidfEzqu6qKCoQRxp505LZOYD%2Fpd8e6OQna8rK6QmCUD3wJPYidcQHraxoMEanzzOQoeEYYiFvAyu85mk7qU2PV2CQhiVQfqcHSi0to37PuGYk6bABn4Zd9Xqg7o4js86ivGw7yBd%2Bh8DC745Ag9FyV9Y9JVOhfyMIrK48cGOqUBZgs2XLVO8vuoe54mGjEqDd%2Bomeih6x5Qhjf%2BkwfLbtPZecwjF5%2FqqVXJQ%2Bl%2BQ%2B7XyBKdy38xK2yJfKLK2zAkHihhnhVp0flQl2RI9k7srvIjrIrS%2BlL%2BzYzEx6xdGczEyESjoc4kTvb22XbMcrX1zZlCN65prNUkW2ncimnNQ5iM6KrrSnvzNFSmlEIQoz%2FA3xdKp6ubFfV6kmmut8mNT4WuxfKx&X-Amz-Signature=bbb9d4edbdfd95e673c2be5e129a5389ff7b843b9df78f8380ec65c14cd0bc44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

