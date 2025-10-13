---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDYBDT2B%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiAEKH%2BisnxWTur426eOIZZVLr6U0B7bnwgrNApjg50wIhAIcFj649Zkxb5soXv35lAtaSlqu%2FN7c1ySMWUyQfsOkpKv8DCDwQABoMNjM3NDIzMTgzODA1IgwpKqV4XNhyKNv9NiUq3APPCNgG3YeFvEkqoWA%2FuNPlox%2Fcslrv6Q0QYPBQHyXbNpkRbNYudwZJ6%2F99Xc5NQmLxYzINJlThVH8l59qlmpJqbhCcrlOHAdDa0ZoNOu4q0hrXRfuhEj9l%2FMvaeMTgG6a0RjlyaOwHf7M76jHV6ZunqLMDt0249zDCgsznC0NHlE4QsbuEutDHo1NmdokOtBqJtUbMD7nhnWWNdzUUx%2F3joVikPMMVgebfsS5tc8CV1aPR%2F94SNPeL0Dfpuh5Fn9mwFuXcO71psK3r7GAoII1xlMz2Huw0YAmFZhQOMgxQyxCRsfEeRakh4SVhGd57tzDRHRjX7JTij3%2FUK1wsytZYGRzX3LkU2ywJpyXHvQ5ndAYddN7oqCHcKRPORn6%2F1RiUMvMzTbIl3zrJGO1ThdlTHoau4RCUxkvL5oy4t2kHpXNA7IaguMTLjR2Cehb8HYf%2BREm2gdZvmGzIW9RJEE3dRbu3AIwrYyroRZmuDisBxoPhn8JcO7W4oTR85ilqopdMVeAUgIgtHXd8dSRks%2BMua%2BJnkroC1KjXQRi3mu%2F%2FTkc4i7hqveWESf0%2FuIF4ck%2FZePy0GEpCyoZlYLqSQTGfK9QLWbcPd0%2BKNFJGB%2BwT45L6xUyXHabHwMZ%2B7zCQ07HHBjqkAe%2FR8RqbtOAd9P15%2BDMcKf4iMv1u8JHZ8CGS9uqGpBmceixLrz8g2DBlj9XLvKBQnWqP0RsmQp0StFlq33770CFkwSUFDGNgoqDXnprQg6pSxxosMY%2BPWcc9vGc43ZQLdp03VnTRYPgIGeXh82JOWp5RebpWLrl1WzvfaATMiLmXzqUcftrnZhli386IxeMqlUojbzBSkhqrXjKTrn6b1hioJ9xw&X-Amz-Signature=fb2dcfe820a3937905d5c753072bbf30f9ccdc1a66895d2848853ddc9dcf2d2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

