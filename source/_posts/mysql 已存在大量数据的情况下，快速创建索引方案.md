---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664O4AEQ52%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQCI4bwGC9ULNDmSaorFu9Bpiwf6BiR0N2J0co%2BEfj7YEQIhAOlT6wf1HoFhpN9CbIsJunudLwBOxvaBwLZbOY5OF3GaKv8DCDYQABoMNjM3NDIzMTgzODA1Igz64M%2FNse4MBUjKWFMq3AOxMXn6psZxVkJzxuF4vINzsUi8YzjNqzkckAXL1ycjRuNbkwqcOOO78jL0NZGkp7VjvY4RdhGw139qIbkpvssphTmj3ppXoROizZVyoDGHU7gh50vN1%2Fl041rhcmxFkn2ZuAv8HOnB7CHDJCzI13l4Dtrq08b%2Bx6CqQY%2FKIz%2BuCxVa5KEagWsYAkxAdc0qdPjVQMeef7m%2FIXG7Wdq5A5mPBW5eQfPZmBSJWv4cVS%2FhesgZGC0L3qj7JyTOlPPLpFlLkZQ0wPnh0gnCoDjVyyJ3SlEi5M6wrkLp6gVVLJIeOGq0zGcT%2FGGz0ktsTCAcvRZiauq3rR%2BRGdKWCA9FBwf%2BjaY4JF%2Bwigex1fAwP44cfuVeFzoLwQdB%2Fqi9QQTmOK4aC27NyAsm7ZkEMZaGnTSA7fo%2BkTFKwQA%2FrJJ7tH1SLr6BM2hsvFZp8byVNRV%2FWxqykW6gCRXEsOQP8BS%2FjoHSjBgXBJQlDNjFZPw5TyNjEJ7Fwignm9hvPjIypdrCWTuewbQYCsw56ITM%2B68ai%2F7lqGDAk%2FU1CdyomBr2gVQsQi5grvcZ84Sh88ffQAuq84Rr49XoIAkSz2Dj4QzMnY%2FVBNhwW%2FYoX8rm5GEVdzJWoyRs73lcW9frYkaixTCKgtLIBjqkAcl4gCufrs2VpQ7CBxMgMW9Nb0GfB5hxJpJ%2FFFzrmY8pRsbGspQBc4CewJR79YjVoxttlulRtb7cahR%2FHHXbbffnRnrXAeWaCv9KfTofwG%2BwpI6YKv4RlJ2H7n2xAk4CRnHIIFTI8ObLhodEk%2BP0qz%2B5atEkVy8oAgdqIzT7Wq8cYVy5XTExke4uXDMWfQF9ZDLC2W%2FMivPAxPM%2FCotC4JmeTRdK&X-Amz-Signature=d1f623dbe6941f6651adfc61905dedd09efc972c067d10842bf5a40074dbdebd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

