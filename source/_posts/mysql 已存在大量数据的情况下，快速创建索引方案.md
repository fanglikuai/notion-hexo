---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UVWU2D%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCV2W6HlFTt%2Bp7iFFG2lw21KXh1Bfe3dqTaG3%2FHY%2FdoAAIgE0TrxBimeSmGtp1Wocum3x%2BPPdHFROItCo8hKlsGU%2Fcq%2FwMIZBAAGgw2Mzc0MjMxODM4MDUiDLCsBMskkvxUDjnB%2BircAzgVpJIlxkv3%2BRnqNMCtDRC7sUBF4tI6JdruCVwyZXrLjmyNDY%2BfvFl2pzuP1109re9yp9K2k9hfxfZmbryRBPt8u8gDm1qMJUbfREX2Xm%2BOKSiUoe1KDeFW1XTVotaWgND%2B4JSjn1O%2BZVWNH7viV0dCvkeVnFdExZj0pwMGiv62QKrbQhnOFqyPekPGfgky4xZYjAAqKoMHHNKIbPhUHzPRmjijdA9oriWdhJnVhyiqkKUoGiTOeoOGOk8Ktw4CmGmoDZpCEaG3yIkduqw4yzBABLWHmFZwvhb2vgTGQoA8OEWTJIUVmrFz5vqEm37qt2gRwLo%2B87t242jkg6pMBNZi20A3Mb3zgK0Gwpiri5Rn1KI4pNGDzfhooPd4mQuDtmhUISg4ed6v0johJcN42UWUe%2B2viL9K5Y877xErdLPp3i5UBrGVMER4rjHZlmU%2BHA90sAK2UhbHEZUbh7VGd%2FTqmbT4XTPt4r1vJFOf74zlc0n0u5LMASlWbFTWV5ZnQQnZuiI2IJ9hhjhxVJctnPwuqqf%2FvWVMeRKbWWqlaiDra9fJTsObDz%2ByXZwAil2a40wUXtM6tzVB%2BgE2Y4x1c0KG%2FdECPBN%2FEvnFGsE7KMOjGh0Ez5ZfWoTuGkX4MKS5lMkGOqUBZYDYoarproLna%2Bi%2FVbIFTiEyEi7ENvHMIfelUo%2BhdGsNduAhdlYvioLyei%2B3s%2Fe2LYgWthTJpRMUvqehRKm4JFKa0%2F9JUzxYfLdpUc00fuwBNY6Ij%2BKYa3SEkCqP%2BPzv4LTFYbCxjscOqyHr0SRa1pIUm8GZkouws%2BtrHG%2B5xEJv41iYUkC4J8pZcwF7lKvDeYKYUGhDGRjIRIb9yuF4K7rDKCAj&X-Amz-Signature=02087d5e2a88fb9ab28b39603e61265883246599ed64535381d97983ced9b2d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

