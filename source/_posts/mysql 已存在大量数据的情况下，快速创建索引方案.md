---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WET32FYC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T130107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBk00ZUExktKcS84gkQFf2mkrDZm%2F%2FKS2L11pMH8G0JxAiBnwPWZR2hcmKoOd2BEBleACUCGk3Sl5vhytJ9WzEcTSCr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMhx7BPb9jS39l16HYKtwD8k9tzyAjkacxk1OpwYbmhWf8t0n23hPvfPL27rRX953dG8oU8GhNgWleKkFGQBpEtM3LxFM7NdNCon7Xfy90OX1DdqpqOC3Axx9WYHq%2FnbSc5dxYEE34f0OFSZrD5bpAXP81LDmlpORoS6X4aSdm6btPe7GH2tlAkPuDDWEqDCkiR3qbrJ47JShnnce5lxnV7979cKiZq%2Fnp2tyqX9eAQLMPXWOIVXg5qeFETrswKFjrkdkg0rWjkm01oDTyFjgYkAoNJ2UND0qcLzxF%2FLsk%2BiueCLO6jIWe3BO%2FVisinQFtBA2zhPdvPQkXADB9ND8kF5PDVPhRiEAXobXu13XbRoBDn6HJF3LBcbDUlbcksL5eGW58PbySdcvqd%2BTQksYYURFu85Yu8GgskKBrXBnTW%2FK3BVdG56shR9dmCSJzsVjfyVG2bPvHtf25tG4d8catbnFXTwH9JpNKVTi11X3FrY8ASzl85TAAbD4QR8wbFqQig36rEwSSZPvsDKFMb6Gh16uKjPq7fnVpdVzVV0hcjevIBMb17FFWzENzcBsVp9X5p3XLCsCV0tW3fSDJSkkOzD81msTPEQUaC3F%2BMMO2VwVUN2zd0DHaDyuYqVm1jwNZrXv2dNtdSg3euNIwuJaLyQY6pgFFkOLAhK2ZkfEY%2BfPH3aYXTibUeK8VaTqhiOrfgG%2B1%2BNfcFmCDYoplUNSu6HxG7xXpBx6A4pLF7F%2BzY%2BIxRwBjNUHt%2FpWG4rTM4qKHkgkRwXQvomabkX4WR9M%2FUy%2FprLz%2FlIms83XeI43dF%2Fv9vOD4RntScXw5L34Dg1jxVB1%2B0m2RVogIwl6LxHFLWzvcznZv8DEpToV0YEc8I%2FAI%2FLP7s58nk17R&X-Amz-Signature=c68febdd4b3eea8716f53f36cfc4787fa673447df7ae8d621df5d41e81bb7cca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

