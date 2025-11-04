---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYUSO3BX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC2x2xO%2BmKeemTH61HUjfCxE3KHJeUysfDC%2BlWhzTme9wIgd0a%2FFqfy91QXqbCJJt5rnj8EFhAflxstSTmG2G7D4j0q%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDIndyxgXJy82ZBVm9CrcAx%2BdSrJTb%2BFSOeyoz98sXph%2BzrAWRdW2DLvglJUESuDWG0I2ped56ZNeL9GjyBsIc2lmT84JimKHpRBco5ZiZxYyt5hH34ryGDoIsJO9IqsEorHebhY%2F4PWROixrtfkledu4z%2Bj850uEDC3D0PFbVuCZAzUns4S3s6Ka7qWiGC83JkXNNT8ZR%2B9XbK8sm7yWlNMz0TDRFj7UDDZyrTvKKOuee6r7FARo1%2F5gNO5x7Vwx%2Fe5lpCzBWZr%2FqkMoxyn6CvZkv1BTsFXhin2ij9I3janKf%2FrdsNcWOvQYeY%2BM2dbkx5H11%2BMzHBUZ0YFzZAuZYnmaNVJKR4u5ZOtD8rHahxgfNEDEB5Lid9dKbeic8cUAD48i%2FMAVABhZHTuKsrsxg1LKoM%2Ft0xMIibdKB7Ca5h15fFME%2FXEgD6Lx%2Bg9t4jndxWJ5k9UfO2Oa9AOACvBiDpz%2FxXUb7NOczy0zK9xLZ9Z9NXEd%2BL4mzaIjTQclkIEsCFHUH5a9OxgeEOQRvc4KYpulheLXjygoUsSheTFFfnIBwTtoA%2BBiBcAm0e04lMvEMkwR219dyZ3meEL6FqnHH%2FXn8EHbMmLjUHE7E1B2uK3oZ5ofPy6SaGkVTPVtnlTPgPidIcOEsWTe8qEjMKCfpcgGOqUBFZXOmWlGlvXOU%2FChns6fxvSqNecXJ6czTJQdcYq5jYvOJ%2B8F2rTydAxZ3LMxL7z1rU9fhNC7KcoGH6Zq3dGVCZBbuupTRp%2BSuD5U6K35ArLkV8ozhKlRr%2FCQ9pzcBvMLPhtkbr%2BA64ivugXTjGtpjCaHlHdTTsktBr8ao5XvHhYwuXNPJG10nmP52nMfgaBVspS6La1%2BV6OxHycRtxwX4S6wyFg5&X-Amz-Signature=a49f9c3878f973f924e081a7a360d5b5589c8c7c8d9ab8a0aa969cf985c42874&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

