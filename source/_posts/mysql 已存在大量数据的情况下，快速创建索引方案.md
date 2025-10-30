---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAJQVQKF%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJGMEQCIB3RspOEyS%2BbQvou9OyBdPxvyDQBNRb1pTJVP0p0WVYaAiADHb0Q%2F0Nlv11S9Vt8d5rOpNt9c3OCl2SqSzJNCTPwgSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJLr2IJ2xzFHzOflbKtwDg5p%2Bt2voxJqnmVa6X5VAU1JHRuUCOwED2H%2FOWEMDRKr%2FGCQUPCip133ii4Vku10stO7d8TB6bvfneDvdba%2BuxKvxGCjc5soV6tSoWDnxdJ7hmHQWnQCAFhiakW5rIeeIi2nVs9rFJP3ULtUdjt%2FHLoOblgkAd0ZiajUH6wVDBpJKBa9MZuqQ5jQdGviDS7hkzxnRM5tFTCyL7GDuI9wxvo65UuQW6689SGsCXv4GIzxASJYIpiaAXU%2FhI2DHXoCjE13iaWUGCmlU04%2Fih186yud5Ii%2BGNhLsePeuGtrcbnFm%2Bo4XlOPIu80U1N0BRBLpxYVPvZnnFV%2B%2Bn5fxAik3o9aP0ObB3FAe67bbD%2BptB4F2ckSQhpDVtshmRnZ6okNIqIX%2FHDx6uwfnnY4828j%2FVakhZIOvQJz9tfYw99ER5IACTwHViLoZrc4QcLmoHl000n3VSOP3tzGQ2YJWiI4%2BZOB%2FFXh4Th9P6kPnqAQgNcAYZhUa1IFFUdTjXhL1YatCbcPk005SJQq8TGaUkwazEiTRaQ4xnEj6W8z9aR%2F0nvFVCm4PQ1CIhE8lq4ztfF7SctdYihz0%2BFcmzhdWY34WnvQ7xkawhqZb00LB95xrMWtWqXG7XxW9D4FCiGIwg7SNyAY6pgGTRreL4q99AMPaqJA8dnHjjcCnodaBTpgcBSGtKXRbDFt3u1EVW%2B30Ta76XVGG5wX2GDWMdHcROOwyok4tl22SM5DwrYpHMyKDZ7jc9npD3IHsRmcCd5H2k%2BJ%2FqvfF3ElUWUmcDaqhUwU7%2F7%2FryZJ4zbQE5dmVwYWulPgANSFCNowTxjLeSEg8aXO%2FDBN8XNvdJweEqVkLYBX4xGajMHPVYkBAxPNu&X-Amz-Signature=783b9a64d8489b9fd1ddf4118eacdf0bdc7359cbdfdab9a7d6622cf53cadfb2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

