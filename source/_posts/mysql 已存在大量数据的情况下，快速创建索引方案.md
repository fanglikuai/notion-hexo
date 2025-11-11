---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZWK2PPR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCICKojMqtLg8WNCjTPf%2B6x%2BfA7XP9mXYsV9%2FJ2vGXrmETAiBjALNTPRVbWeHVoSFwOhOMGHCNtdOJWF%2BZhVc%2FKpMeSSr%2FAwgUEAAaDDYzNzQyMzE4MzgwNSIMUzQapnTiAud9%2Fk49KtwDvt9F90w4IrEPUwm0vPTmx4Y11dtHJsYJSFRCG0DAeeDwCUn0UTJI%2F5IqPRSbJYt%2BOZEBIivCa3iwEbBETBxaS4NL0KO%2BpMg2zD14ZWq4Yah3EHJGqBvcuJ84a%2B4BnlmVkNBNQxH4aVzRFEe8UvGrcp6QITMd3GRC2mIseP1g7Yi9F3ZFdzySylRoqwXiL0tvNyPYmrv7qijAxyOXfKmqwIKrxOqC2GcZCg0KF2W%2FAzQEbhyi2tjWlD505E4LEcEgU0ccLRop0yLqil21YKVtb5zu1o%2Bt%2FTCzh%2BdsCBpOxR%2FwROOq3jsN2Urxjg8QeaGA3eyuHPb%2F5TRZwzHaWk7bF4fLCHuDuIteqFvf9UdjUhSdqx%2BN9KgTLAlUtw%2BEVnJ2PMXfvx%2BpAWD%2F1EgupBmr8VYbXH46EiGx%2BxmDI0LNtgE3JDjdUH3pO6f9WYYMKSZDuhCmAuzyjdFmMBDG7ZCOYAyQXWu8TJJbPaKGhTfj9TrHrmev9J86s1GvIxhBHPMozgaCRo2x71nepJpYco7OoRWwVgot6rgZqtpZZpMfMkLcZkjnAZ02%2FrEnkk3WypAmZ3gIMia0q510w7fQC1gd5HqT7m45aM7Rby4gj9g2%2FQG0ehg3HeOUdfqjG80w8cfKyAY6pgHYknp4Sm8lHJ6yk4XWRupNFxIwLH7O596cIWHATjjVmiQbFoGXxY2m5TPlMORYFzJZDhZAZc4xl4x8apye96O7EaWiOLAZe48QIiRYn6jeHGsH7KErSnOgCvVbq%2BF1g9caRuQp9rORvIv59sIk0A6SJCd6x1Zaoy05t4YJDVZyMcKXLyKRXw2dPsn35whiedKn9MuGJwmSXaTcmpWB8iviTANjmU7r&X-Amz-Signature=b6675eac60eeca2b4866ab2b0b803141293903943adae239240d85e50b6ff288&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

