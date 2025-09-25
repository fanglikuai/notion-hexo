---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2GC6QSQ%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCg0ohHdPaOzH%2BjyPG3fpKsNygxKnirRMDxNv%2BZ8tUNRAIhAN4UfnRzzFyQ0afMqedxmDm%2Fx%2BFHSxctaaVwosjGX7aMKv8DCHcQABoMNjM3NDIzMTgzODA1IgwbE0ONARHujvGqQ5cq3ANf0bQV2oKCG3NiUHEjZaU50tIhVbjAHMJquS0%2F%2FrhgHvfwFpYHpQ4adlkM2hYUUck1TK1Sb9N1ggF63cKMVHpESxLf6dAZpgSsM02K7crpxaHUaMh9nDlvk6PQbm3VbK7T%2BoyGnFBf57xP5b7A1EZcR9QA0vCCGCtJPZQ7V5MWu%2BNPbnWu%2FQrJ621NxppnKMsee2dvz4%2BKZ8C8wZyyH8mH7Lea8PTX6PMcmv5vTWqJRmViJo3H9agb1o%2BEyt5CW6nCArHwAFbeerBTuQwkJsHvR76xK4GE%2BPAO%2FGMR9MoNInRHJ%2BfkCV9tCMsTdev4KaZajOmhhYkipwSjHckQYllTxJcXEzXpOYiNgxyHxSLtXKt2qf1pdbOE41dvD0qN2iQ9iJwVu61GxNWVpmE2Re6QtzN77rF1cmBxm2gOpXUzk0qQKCv4exkYqjKGcCaXCv3%2B9bGj1DQ1CgZP91UNsmtQ%2BaCXhyaEkjF0ab7aQ3SELsajhn8DD23%2Fvo8PDUXz0HSkqx49JPYbhFOVtYXSL3hyHJ8Z09oLFKYC86ckIzKB6M6uNAqaNjjSKZwas48cFReJneM7cR3%2BSwXPRVfq9A%2BgHG%2B7DYcDU8GDycmULUF04JTtyZIThQZEgepd6TD4itXGBjqkAdSCVBGvwgGkNIL%2BezcuyMPb35%2Bs91zhCRrL63eCmqDA5W8VOi1n2OErJsXNxiil4Ab08DBVANEEu42G4PPqqFt4SwhKeBXrupqX3utEHloQqTbCQGGcCm3NPFKn%2BfbXr1zBUQLfnk6yS0ZWbXNU76aRSAEwJ1ApYmgXpKWTEgzppKav%2B2nvE6x8fW62%2FntmZ7KpltQ1DkYUM6xHXKjPMW9pvAHG&X-Amz-Signature=4c59b3298dc38728f59828bd2473932e9d3e2b387a6704ddae3db647f12cbca1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

