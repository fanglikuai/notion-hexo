---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRIRXAZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIHbTkhMlj21EjTIJgd%2BUWbIuGCvakg41P47a%2BTlDh0S0AiEA%2FLvTRKrARyRS5RTHPvDGkC4%2F%2BT7TL7WdilIYu49iI0Uq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDLGz1NV0WtFRWnSAPCrcA07eh3%2BivmO8FFJr5ZFLPNdjbnjApypf9ckUSNAIwi7NPRsJiEmLPfku7bMUZXviYZNLNUmcX5cIZDWfpnOFgoLa%2BGK%2BmGyPGJFa1nR4pKGzJ%2Bn4LYulLvmtIs819zu0Zl0pCIyLBDyV%2FpFPhf0SjWpGcZbP7f4dkE5yw4HvwzHl3wf1LWTm5kCsamh2wYTRf9pH70QLhgxqL%2B8RKia1z3F5SGyr3JcIG1upva0VNQKxg1ysiDJ7UP%2FPYgUnG9srl6aYWXKzzX48t15qfoUMKOwhC6uZtHK837FkVzwaMJ0Qf0YPz8CRhD0U%2Bf4DlQlRVcTApn18SLQKLlu6RmozGm6Qe5dPMFJMTqpnkh4dP3qp%2B8PrRnbai1PPxVeRtOgPmPGVJMtUrxNEKosnibNih3CUKhqyvj3lNrw9la3r%2BRnej%2BneyFqlYWGGeJbxs%2FE5gOIy2OFdTUXM4PhmaILWmsKFLSLXHM2%2FDFUuIfp5UA%2BKnOQBb8BmTikbNTQ67GJIlcPMFH8yGG476xaEyu%2BYnOQiXHVTWU%2BB50zOVsSouL2m3xHM%2BN%2BejS0W6ERktoQfgGaj1k%2FLIcsBBTkavPS%2B6rZU0MzhLL73S8v20JI2kPvS7fCV0G0hJDC1hinwMIO7jMkGOqUB1fFW6LJkFvEi4KlM7xqihspeiRFiBVKO2jm5sCvMKCy1TdvbK6m00UxgBNtICswUR%2B9QJRfT1Cet2SOz23zHx8HIG8urQJWwM9fhoFJxm7pMnbE8ImODiosbPTZ6Im4M2nvmgeuuYwYn%2FbjA23dND2%2Bc%2BrOaQFZHn1htZmCQ1zepcSSfxcLmby5tW4SuLpSief8SZCSnQ4Xb4fgD%2F0n7QBBTAqFQ&X-Amz-Signature=98cfa9bc4848b23cb20be73ce01e4653a3b9b573e2342044a0051896ad1a6b0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

