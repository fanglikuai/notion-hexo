---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGQIALZM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDW3%2F7SmNiJ6zMG0BhqXEs9721zboNmbeSIBM9rTYe9sgIhAPAOghTHKIVoZHQN4NcZWcl%2FVWXsUQiucQjEf2eqj9EIKv8DCB4QABoMNjM3NDIzMTgzODA1Igx8HjROX4CWXQMnEHwq3AOIoMIvnZemNmFMr3H6CJUVnw%2BPrYef5e3KaBIbSKlA1rhk18uDc9Tg67OS895DfcqVIiODhepSq8VSbW4B95WKzW4jQ1DkhoQAV4SUq15PBIdBCeZq%2BTyMqDfllnymoGH3Dv4HMSCLDEUQad4VqY9eEr8bEfTmDholh6P%2Fgr51%2B3TTp%2FME8Fq18B3HLOOpZXI04ISzP1yUr9YQCvzc6eVowcW0BgekEqoGYRniPFewlTZfKgOWCdreLr5u37IpHdhzrl%2Bx0w%2BRm5X35htKzWX%2BExDhwjDFqQLU3nCkzzGn4%2FYQZlvB2T%2BeZbrbQJRguaTRVubXtzuFxh6CSJ7I370cy6MRAYU%2F%2FrPYQmW4%2B6xjlgEVpB%2FD5K6CoOjZCerGEHE54ldM1gWrW4hfAvNdDCc5tRsJm8ufA0%2FBrxQZZ51xXOmh66kpMhnCZrTREAiJkiGuf%2BhgBgA7CBzZX42tj33AWk3%2Bvlb1vgO64fbjCQO0qbxjeKbYwciLyK6ugBy6S2RJW%2B5ubI8yRzs8IO4nzn50KZScTqJgDZ5Ldza7FzVielmwan5jliKCEPvgK4ugx2vYV3tfGp%2BcoF3BSCWRLF2mbjjBvPO7eWNcbXoyGKwgmeg18GaAv1zrM3KtRDCIwZTIBjqkAedsqJtbIQq9Nje4qnRcs2%2FTsgEp80i9cyJvIHFE006LqL60DBI1KC0XZnjnbNdS25nQSqxfVeOsdJ3SMxqD1uIQKoAIgXEReXhT33gp8v%2FaZRNqxmCSgUWnx5x2AVATpEA47I5CcC%2BfOOYjNnPsRQbDeMeXXg2hh5a6cpmc8zo%2FOdfRpRfSVzqcg54PUo6%2BOKdtgHEdY7Ge0mWniYBFNmWrMGQ2&X-Amz-Signature=69f39392a76310641efc02bfc06c07671ee25fee6fe03c30e0798afe4fcfee10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

