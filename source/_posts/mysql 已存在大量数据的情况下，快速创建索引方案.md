---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4AJB4KS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIA0K%2BNffrDUt1n0NJJJUGuUZAUao%2Fhstpo7l4T4eMg4nAiEAyF74rs5sp1XI0etOu6MGjU8IAwxKWhbsRgTXtS%2BwU34qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHABf1DqoApLcg5pSyrcA19kFD2GjvaYKB2PsdP%2Bk9qYiIypSaiE%2BlXH%2FmjarrvFjrGSpnfFy2AAT6HFgn%2Bc4MEgqEEQ9oBW5e3kPLVVAa8HnZ3C%2BOC9s7hlH6PAP%2Fkp0KLsEK0P%2BcsDKUh8s46kAryYso4WpbiqHEIIytQMw47DDD%2B9jACJ09am3RSWCFilyaX1b7fDF6qYcAgqlVCJW%2Fw%2FU5QfqDeq8XfbmmuPrKeSG%2FNTZDqXoAg6gSPNm1%2BwZ3O5MjoCa2Xj%2Bj2E5VbxiIKCP8s90MwY0nqlapFZa0KYtsQgPYbce5273bKUGkxc62LfMz3U2wA9mY2kEPPy5PiplhpsnTjlMy5RkHWa%2FAI%2BWGaGElVWNKcRTa%2F3gPrEMlPvOnQr2j4qp4xWG0LL4N1xkFKfmID4tZIWEFRBwk4jU03wxA6QjLu8E8OQs09UesFzTMn6TjRqampENBd66DsQ0WqRuci%2FBgs8tJaXQW0ZLxyyGD%2BakCySXodbSPEX7eJkfzQTOWceND5F1t4sXoergevStBx2awQzudSd5xWi5tHVXPgmzYeQWPyDOhD8Snu6efkQZwINDETZoJRa5TgkiH3YDBIvdBZLqocpsNsxLXNsjqMXhzPqQzinJcn2r3yNwJ7rUFJbDJh4MJ%2FQ6MYGOqUBv8cnECZvE%2BiLkaTHNY1b3akqlaKRDX7cAAqvkyQc41T6P%2Fkr3S95VpIi%2B2tIyQc8svNUyMHhYe8BcC8M3xlPxI1YAY%2FmTmW3JzEBt2xEpe6Oum99TtQFdtxvSTV7Kr%2F7VBLWAm%2FItDv%2BsBuZi%2F5MHpK%2FGrKzAXmrkXx0SqkjAiUgoEeeFGAQ%2FvM3aBb9YsZSZlRSVgb26%2B8BxswAk2S%2FPvA9Gje1&X-Amz-Signature=ebdac1b6aca0b68677bc1624eb7d94e78b5222f1cce6f240901579e44cad03cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

