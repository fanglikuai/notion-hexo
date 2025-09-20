---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDWQBZK4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJGMEQCIGsxXTEnxF4atQMxy4MiBnPBwk9EYaYgxFGovmfa%2Ft37AiB2%2FoaKvM4cq4%2B4xWqwFtQpnQk%2BmMzIMLTKe1FJCjg5bCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyUkThqFB1wlg6HOrKtwD12H%2BM2rq9GGQqk4gvBUzhs1gPH0K7RPd4MIgCoDVglUsJ%2Bwa0q8AWawbcU%2B0B7nzsqfLIaS49hHrHMFOcRzac0ofmhxej4jFRkekacKSt5u2wUdnR%2FMgORBpbkju%2F8hpF22GtZVX6ikTZhbxAVGaUvosOAdLicbA609hB89yESfR53cWVgN1fSSXuMf3bbeocw7XCWFrcq%2FXnzYMGxgHhP7T%2FxjZSe5uhWYbU8qXMJr3XjLPobtOprogVy94yTm2RR8qcrhoEGH1xm9i3Vd5qm22d7uuDuN3hsVNCAwpd7w0YBfIaE9NDlFFm7drBq9QlvDRLoA%2FsDGQchz66pML4jYxf4oQTl08hqFdfT0AJuLwpfzxcVDNegt%2F5DJpbpj03thn9uhK7xGB3nrz8gNAqQe%2FI0ffiaLNI%2BXoONjpIraPJR9qLDG4wE%2Bv9GXCULJyDc7MUdHG0JaUfvQgQenkZV%2Bn4viKVhyAfMrso1m7zJYY9y6VuRs3r9BgBcRmFRHr4rjNel7TXbOna3zSZwh5mRQMPrB7rFB4NpG8k0Pcvs9%2F6%2BCr%2BWQGvmm49jW4xBEtBNrFhLl6O0G%2B%2FyoxLoxkrYX6QAlWeDa6%2FnRfKJkvCJM%2FdRqIOQTsP2ydeqkwr%2Bq5xgY6pgEROY64sSUdIuAFBlPb8xZEzwPeii9u4dNsPzTnyGz0p7zmSOO9gmURrTWAboosNhcJ17OvZ5udgCrsfU%2BtD1kjLeHrf32T6C4DtG6uYu6kdn%2FF0tV2C2oyU8Ds6dBe8rE7AeHgnDEfBBnkHGdKa9eawIBRABoONO4Bk6NN6bKDPhq%2F%2FsmIXx%2FH7vAzbvRKXPBrAHTXFBfwvLZVthzleHcoaPUNah1C&X-Amz-Signature=534c7c15c02949062d239eef2498f3f5639752f5b0b32a5a36381de60d43748a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

