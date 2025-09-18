---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKUMN3XZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIESVOizWbIA0%2Bf0idUb8rKJSfL%2FMevK7PNZXRKMZqfVoAiEApO3VZgNdNV1e9%2BUlftj8z7qmefPw6m5Z9KUZgv9CE%2FIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH87K1nPQt3%2F%2BfcS6ircA%2BuuRtUSzAMb5mro%2B0K3zCu2r8zmflzYPSvHHEv3jIGljJVSbMTdBJn71CQSigLWt9ucKLFoprnfGmgc0j9hx0V%2BvE3HmoShMs%2BHgs5YSV76gmAB1FnQ4G%2Bni1apt3WlYrWQwLcPrITPKk7dMkU2BMhth3dW2nsKQksJTDt3KiAtj0xeUG6Rpd9dTcvlryeVuonSeq%2BIvQ7OAtaDss0gPg3XG50DXWVOi2UXmMQ3H9MZCrh7i0h0IW8fpzzu7%2FfBwH%2FfGQMQqTaM%2FidshezQpDyYUSkW2cCzVm4idmsSwXpktvSoaQWfSCF%2B6EWKbuqjn%2BB0rQ933uwhjOtAL8QsccN4R3DnOMbMyFwFJ166BBeKVs8IL0NDgz2xgqtnBRARTNYV5W3AdyXPt4XpiK%2Fglz2%2FpkO%2BMJ5JS8iA45PTw0P2OeL1NHwS41mo%2BsbZ1Y6w%2FtXrR8ol%2BixxM%2FiKehoV%2FTS7TbU6Wqi4%2BXGX0mYCepU%2FeM0ZRQVFXoQstZfrtdo5KGjJYRDr1RfWsjnp0g86pEhsAXK3dWcko8LjDh2MnpN6CU66TOUVo2TjXvJKx%2FMitJWzMneEcu%2F4tNQQyyD%2BkZvprKv7zOuTj1C2TF4wdsZdB4ELOZjyyGTiefbEMJTZr8YGOqUB6SQEV2FB6n8B0E8TsVBth9uKV3ejnrfdU4DV0Sp5wZUfDYQRkXDIFomIDKQD6TM4NWM1Y4q%2B3mkYBENQR9OqfgCWouLxlcb4eUppkr2262FNlWtbjBa29d%2B7aU8O2xTGttmEI6Gn21dSEkuK8J0%2Fze49jajq%2FkjNMfku%2BK01KVgl6Jc10fGiBjohkmnfHFVzOQOYoAvUBDXXfnPPVIL5lWrJcss1&X-Amz-Signature=0cd886dbd175f371e8a54e9471ab2a6becd2343ea830e47527a24673a52225c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

