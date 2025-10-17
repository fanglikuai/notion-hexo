---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664AD6ZWFG%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD2TXH64zO3FsLp3V9PzSDM6wl%2BrUz3ll2oEj9YBg1LiQIgW2nojr6IUzQ8oAL28Zv4dujF%2F53NzWsQ3ZaSPTZfPKsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF0fiGoJBW5c4evPYCrcA%2FxZT0Q2MaDkmgVJwsVPW3w76KF9aZFS5OGNLGWk4K%2BiirSO%2FdslKoZsanOTe2HxE0pf%2FGbePiJjjicYDckwQSgSbY%2BLc4t9uT1SW5S4uGvwhHrVXzBOkhVmkbAQdhk1YYJWkw6yB1Eyx%2F5yMgfvHpsDX7FEgKkCdyKVKj6OCf6NkI5l0zufovHenkf8W6BJLL9D8BAMb%2FAcj5iLka0Av0AoChoFHZoeWy58QyphkBknee3eeqy7iCdOpZLskp6p6fTc1jJQYc1UbfWnowTVepiBPgF7CdRaU5ER5TpmFHEsOFoLMZ3qtXljQYZoL7%2Bifw01sDVRi6VXzga4VN4%2FczrTOTseywxCsXJL21j6J9WcDRYlZ3WkknPuLCGImY9yplkVDJcLW0kBgk5p26PdGPFnqx8V4nmUAgfAmMidS%2BzyGb5ji9cQqV2uIp7Bb0J2YSAhzucZ4BmIcYsc%2FlBHtaL7qXxSjVEHVKnnNGhHwoe0ogOM9BNv5%2FjfYzdg6gQllJ510hNRcWoHCO0mFNA746T6xJF57uqcDNE0pr7qHauivsKiIoRB2kv0lOWyyQZ3%2BwGsDJVrmL9rYcVFKzB%2BGFCcZaUBMGwOADUPfgfsBYKnPdiSDYPfYTR2mTeBMLXBxscGOqUBhArngI6ixQvhOAibVngJUHyA04yxal%2Blp4xHN3Aity7GIyE9scvQABPQkhsEFZyORKRucHJgomMToSxaVwT89FPRJhUsJAD40057ySrnanm%2B38GnfK%2Bbbxci%2Byyj1sarGhhkcM524g5aygKH2adFxrzOUDtUqbckXAUnTbM0oJXwyfvCQzuXKu%2FGxQ9DKQlME1OfPyV43%2ByjjITm95rsOvadrlzJ&X-Amz-Signature=33b96109e4fa61c51f21c6bbdb4578e167b2e6df36472e3b7f22e7de427fd6a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

