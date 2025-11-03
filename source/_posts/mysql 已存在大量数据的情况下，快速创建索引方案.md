---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5I6ZIXD%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDkADk0KKqvs2wnToCi%2FKm%2FRpSJfkXpgdZn8xsQunOzpQIhAKaQ0JWncJiu%2BAliPLcxMK1CMkM%2BahtFwlIfsOK7O%2F9AKv8DCGUQABoMNjM3NDIzMTgzODA1IgyZUHXeSgt1UgCmigkq3AOgOKuN5%2BhFJ0GXJISXQYfYnlvKGV0WIUIAlVpR8Df7Ld5dn0U%2BfuHkv8G%2BhE4QMjF72OPx2Yl6Xr4RJP7FaiIz0cIrYtcWZfYyUWfK7RX7gs%2FWPrvwmghm%2Fry8qVAf1b8xcJ7xR9h%2FT8E3u2n5aUUy5c0raKjU0J8CGckUPsNmnQAJjN7b%2BTTfoQAb%2BGREXE5iCxQlDI01NfVUP2UkAXFvSLMZVreyUKCSzrwODDULOS8mMNnyvJNYp6VOcHson2K2TCVfWvfluCAeNhclW50nyn0xvKM7gGIh0trBQXc%2F4N3CET9R4vh6w6VGB5QXpxn5EdRgcvI3r9AkZf6w0RSfHk8DtaNPTLihAFqQ2xZhtOHg%2BLinrwHPNkK760KRNynz00YqhridXbTBAvZq%2FqqEPV90gG4Q5tAY%2Bvt%2Br8qlr7K9RKs%2B0xNAWuD%2BDp3EFwFHCvmp8aGd59NRSVGxoHj27RJYZ3uzPYTHuBPXfijGpy7p28o2%2BiITeOI%2BtQofcganiV3AMZ9c7uYKh8GlCEcVKG7CRkbP5sYFG6jccGyu%2B5V8x4xWeif6prtse%2BoSAOuJ6NjOvPKBZZIrZOXE4BdlXojAvEYIWksz07kt9R969e0cxkxqxNb%2BckpdIDCEk6TIBjqkAcKGOMv7rjMrewlFmA2rqzSi7zqNSavZ92OmPJ%2FWKN%2BwFOqzdPGVKlgAlCU1Pta55gyQGPsCyVajc%2FK07ihUyPrbr0jjzEz9LfQ9Yxu5TNSx1I2iJz8avoltb8esgSVTCBRlFDcp2n2j%2Fc%2BKrDqLwXzMs%2F0mJeZqlUSA9cpYTKtKbswaV6hsPVnsTvpg1H2bupPT2OEHM8JHfEX7oAAI8oxxf4q2&X-Amz-Signature=fd1aa9d413b90a07366da81ac19829d495a4c3cedff4fd9f47ef87fab5c686b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

