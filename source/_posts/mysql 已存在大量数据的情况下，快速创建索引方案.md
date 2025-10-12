---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKZ7B222%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA09FIxy6AfrpnEDDZyNhr44hor%2BP2GmlVMrbWrJ5PoIAiAI%2FBf8xcwO0lGCoRLlpTaLZDVz98w1%2Bf99OXyxiG%2FU%2Fir%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIM6eJE%2FLQPNiy%2F93C7KtwD3alpR9PteFMaKCXQClvvMKnu2oIIxWCsEnwyzFcoBgA1sckodkJ%2FSqrAVMZ%2Fqn%2BkJqBf7IcWYNEgtQwjS7FpHe5%2B3Ednt9tEgUFR3c%2B5dNzYdFX9onUTKNO4aWOs5NDotnJ64g1B2Y%2FwQ2Mbi4aP%2Blt1KZiIn195vioy5eYQUBNxqp0wiNb9IbkhCxskFw79FgVIZUZmioLwlse2S3H0acva2ZBsx%2BJKD6653HSrbHT%2BC%2FrsYa75L9V5tXv5JHc%2FvD0EW%2FmP3O3RuaohlmeIGZy%2BeJRFIDSxWsqulUnBYoRjh68q7YpBVhUWXaedUN%2FIcAG3KLCLi5ZhZX4u4TJtOu4cq4da7PFnhTpR4hyF1X1hkzEKIGe5iDhqotUz%2Bv9CrWdWPhlRefR8VilKSEJwS2k05Hnc%2FsG1%2BKlyGk8GDQ6LSdPJe2mT92jtwKpuvfe91Ji3D7GX4vD25XKLbthn1Jv7%2BZUv1guw3t0%2BPOgmQK8UPZgqwQL5HhsxZlsb4wHWFeGDMfZj1wYvmXvYPcH2lAcGuz6OpgoXLoDxKP0aFLvlJkuEgTFrKDgNRKdqmtPIneNiDU6i1i58gUXCpnJhXeFWZvVOC1YZRvEml3wDPAD2pNNWucxaTLN3EUQw17iuxwY6pgFAFqP7ZdWNWbKEUO8bvvwmY5%2FoKexnkr%2FCs7DY7YqX0NcjD3oSHTuKJpPsCD6ygjkPNE7%2BvgGuBkeMv4HV4PYglKTjFIcxNhyKpG8kdgS33%2FZmmrDXHxR2MajOmUaPnwfjEYb%2F4zM9TNxlJHn0OqFTf%2BHNqNImI26FkatNGCJQZG4JzwvPPV3dBG2u4zIkV1RN0jdwN9TAQKIcZ1DJhVqoeJ1MUZaw&X-Amz-Signature=c84069cab5e0032ee2ae634a859b57cd80079263318a75e9db2cc78d93ab1d1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

