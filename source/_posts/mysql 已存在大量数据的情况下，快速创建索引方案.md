---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KSBXMOL%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDIYQXtSg%2Bi7I0%2FDdUjzZ5UsY%2BkX3%2Fdxnpv2HSoOP2sggIgM83LpJluZEnVVZO2a2uHVvrXtxF9hlxQgqVHH1%2FWeQwqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOxqhFZLhmOUHmilircA4qbCLVx5Ne5RNDCijyKtI6ybM%2FcIwm0kWb874z7G8ByRdVA75R2lojcqQ5gB8fyp3%2B1jeaDh770em2YOHUExXkAG%2BcV8LMsDiWvh2qHjXqfGYhYeI1RScrOBGofdAn8TAy1OsmbJCC3P19Nj9PPPRd29c%2BLmWmKFDwb6HcQmgDFHjkr8jMwj74Vp9Mz8vtgDOTLd8q%2BFiCIJ892W4LP8j%2FVOzRB%2B%2FIHUJHjQow68fkUN9EX44zTiSYJgTas5i8IhXS%2F5pEy8%2BTXM4PpFgOFqBntg2JBYJL3z6LprmZS8WMMaMY2VhwmfNW6BMbsCLAp8Fh7VYSfzsKUVLdyXSporelGyC4Nc5kdKPd9SOCXDzAP0TlNjL8unmjNKAw8aIQHsZntFmlnUxNO37u5YHQGbLb1hqKWPeuUFU4MUsHahyXgq9WArQVIA%2Bp3Ffcdej%2BcCrAgZMtaxlZY8x0BAQIB1J1Sud3P5fA1uGfKPoM2%2FQZmKng59EaPhNC8lNkpcyUVwVHtP%2Bz%2BKJZkUtoJui4c%2FJyBST3iAAhHbE%2Fd7r0s4P5%2FmDGxImJfeVWyJiDlczS9gqYxF8lh23SLdUi4pX6uohbfz7ApbvZD768VX3i%2B3k6PNRHaFOPQzSKQvnL2MPaV9cgGOqUBsgCVYjKDH2i1FhiMmdKqQXyftKo%2BICQhcc3SffeXzdVAL5WOzusye8SPHEeO%2BUxV4xAByfiRmx%2Bbe8xFCAekakEdJS5jJeMJWRKXOlhjTfFXtUUm2yRDxNX3VrRYZAEyGDzE1ypYG1IzSPUK3MiSbAhvJ349Tabsl8OlVd1PDj0sosmy0dgyosquJHF3I2lSYTUKLpvEZWQaB%2FKgGY2qJxTBa%2BBR&X-Amz-Signature=6aaedbaacf6500beb82d1ea8894e44b9f89138fb35c756cae656386963fed221&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

