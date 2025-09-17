---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZUQNRA6A%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIHsBZI%2FdWcLVmm4sDbrASprF03DDJj3gv%2FpY7Jd16rxjAiAk57qt3nlBspL9GkGgzZnkiuD2ZSAqn3l03E%2BEb9Ib7SqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMb7tnqysV4Lar%2BOKqKtwDLtjY5ufDAMFD6PVDLG6pZAIc%2B2UqC4Rmo9%2Fu9ayZDl22bfz4VdBo6KfX2BqxqzSH8x59XhIH7GNWnsO4VV%2FTLpbB2whKMxdmdZB3UHh3g3oydcEw0EDE3o8JO4vkKQkOcrP4qIGGqgXA79xL2204zHqWtX16aNlOY8iADWg94MetcCdAd33VRCYg1AZDY7oHy0fbik7Sws96mIUegQAtlpPvZ5zKKKuud2KlUUZamJMAQ5vtax5ZkpvEhj5IqJHdU8ByuOtDbCT%2BhkUH2wCytarQA%2FohWxhwQ0EVlMb3kJ4dBLo5zVvtQwjBHX8hPB0ovHow9Wg4gpWnIs40ZXtozQtyuNAWaIcIWyzLebbdaoNu9LWywXh73CL974wXf1vGDaQ7EtCPzTZsWZJmdU%2BOGw7Vv0kq%2BQJXJJ1DSVK%2F5JlazZwhaJ9yrfpLIW70N6i7IywrHvbdPyVWGF01MljY4G8L8fnrSVrRujZeHTz9Io76FtQFVFJeGgtt5Ga4ua%2F5pf5vNRPv57Qtlo8ypw0Jh57rkxQW9W7MNxB393qhmvh3h0XQ2kZY75EwBEYgedo4dN1OsX46odgy7O6sAVncVoUY0Plo0CjC9RWcITK6fwg8BnwHnUh1jvCONAIw2empxgY6pgEH3bwGL81i6n9233sC4NxXp2eHmdplnBp5mrGYWxs7wEkxiZURfVUhaceDc4cpY0HcHzmH024kUjFycVadAQ7tLdWblMqxtiat2b1hiTEUoHu19Jd2vvDgc00xJeiDrN4%2ByheeT8rODZ0sGWv1UkaEkqYelCvlcnFmA6dWfDtLI2vnoC3ROh0QaE4N5Ah%2BQ4sO00mKgv3QcCPotwuQ3uSEKYTTaR9E&X-Amz-Signature=d8576fa23c952f2b91ebf0e8d6c5605db5db51658f4829254d4490cd43ae56af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

