---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXRXY5Y%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB8kAI%2BtwrPXCAhCxXgzg0lkGHY4tO2NUtfxX571EvdqAiBaP%2Fi5z0EevPDW%2FjsSKGmSxhSHOgiQ5lwRhZEA4Wtzpyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMe6WBOYwRdBHYxp7yKtwDW1yLBg%2BNo4AD3oOVy8WH2BVlrvw08FKgIsS64VMxuWZJtXnCSAuLCBnMiZ03F5UNHijn81w51OZJyMosI5re0p3jNK5ayqCrUGjmFP7%2FqXnmfqHhWmCTGsHBOxnPg%2BwFCDspb8TxkHuFzI2bwGOHpPCWd5wwgBCp4TTAmEVFZ3TxIUFRZxFgV5FnDZiJgq%2B%2BbQUC0zz51iBLz%2BQ2WBVcBwHjtIf63v5POROzWP5mVuyJuBdaH5E4JeLd97xMErk1Slz0XVt08tfpDK9szKcVOMm86ysrRBfD0ly84EC7PHhbY3VbHkk%2Bwqr%2FCg16a1DIb1jLVZZ2g%2FpLIyAryl5tn3nt%2FknEpfvOndG9tiADRS8i%2Fy72dZtSzoHJYiozH77uXoSc3EWrtmFpqq6OR70kcVEti26QFweEcbeGWfQlGCpgKR0xkYlzURon3YTr%2Bi1EcUvMr5UdppWEcbw3oHYnLfw8oT%2BeTsEypjI3358ySbKgEDVXbnXXC%2Bt6j9j243MMcJ2vLSgjCJeOzQHE3hE41o%2FdM%2FiPez3dODXl8r6Pfcm1sLYK8TKwLnyTLmMZRXSHcizVR1NRdaMQQai38UXM9LyLrTVGzsL994eM944JXsV5%2FSFnMjkRrIuUiO4wzqTWyAY6pgFuUn%2FPlfnXNqW7tCDWSYOrd4gSPwvIt%2BWmk7nRwrmAw7yPx5H5EHuLMAWGsq8BPv5fndhNAZeFNw9mBEoZXq6rcAVAJQv%2FUpRkkP926mtUuA4lL13dFBWAYjHRP8ixaWR0OiwREFkTHlWyPXEK4lL54UxlW%2F7Z9TWWtnCVwF%2FJeimN5siELhsE8ysOD598sa25UKzlBfxapYmE21gRO3ovd5Oox10N&X-Amz-Signature=c0912ac8b0d217581e74027a1e157d16d563e14581e2c1f21603cf7fc12fb66a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

