---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEGIDGXG%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T140101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC5Yeoi4k5V1m844BSJenzT0llBVTMMZjmukZqniObWEQIgHAoCtxGMidnLETBXkBLkBi5E%2BQmwGUV7IpHA7H6Q1kYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAeEHxmwRllSVa%2FZbircA0IoEmM33UMP1pK7c%2BkFXuxR06J4yhXgxc4LeRq%2F46bOzJFEJUbWE0qVV10cgsJ0QI6GqvQbQqRNCVgFIRUS%2FpLfJzok8yuxVjjDNuVTSSeMhp0ctvHZQslv9r1CraF7EkzcR57IHG81LimGoVty8NMqoyiTlvxxmymByzUOqG6nHZDiWRY8eVPPWpQofwHZgVEDx5N1jlyraceyLHAqMwnJ48BoKiBlUqOA4pwlxhUwC8qdHSURZmURP%2BcQQ%2BZik%2BYRvhQxfzLsQVaMLEL29Ykp%2BgQMwX9extYTxaN6rmJcavTrIFTM0GNI0egJLRA1b7R1qnk7N4AjaLmkgY1GpJN2Ijn5Z%2BWixFTO4tOjfF17usaEv%2BhonU1%2BRe%2BzvVrSLTO3OIredU8g0dZ1397o8F%2FNWjJ5A6Mx04feXMsdcvEd3FDMkHDUKinYnB82dWogcwS2CZV4P3BSDgGhj3CobOnKYmly11yuI7oX%2BBxTLrm%2FuxIOQJBzJJkQwuyyDY543RguIOHi35LTh8TRNYedBMKNEc5RELxYLMfy4pCAzXBV6zhyNkSrqSQNXpm2wIUsDZaHz%2FZ3QJVt1tHmlCfvGGySu%2FDV9QFwnrH4Ud%2BjVlY8GLHObeFu7lR4utIbMJj2l8gGOqUBGeh54rigMc7wkMoDUgA5QFka1BgpLrKV64JVJ55hZ2VgiNqfG1T7FeDjGFcKxdo53Hr1ir0ut1aBWIvG1AdBMBWJPSkYB4bO%2Bdox9J7sDFsE5lFC62ziOaUSrEB5uIxPhLpJVwn1HRhOmoo8QcomYsnt95sSGxQJQ9ZDj5hjggQZ9UTm6PLnI%2FeXV4y1k4y4%2BqE4ek1ySMoMJ00%2BuLB8WE3pbdPL&X-Amz-Signature=e4e5d72a91df4b104cdadccc055f9b0b70ac52e767a2ac89e15604b09ec25159&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

