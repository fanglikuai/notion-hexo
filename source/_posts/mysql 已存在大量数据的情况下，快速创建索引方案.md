---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMU5JOSG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGUIVHpbC0KEbavg3wUgR5edQ5jDnMC%2FS%2F22jULbu4OkAiBT8K923XvmV%2FCVahd5Xk5ZWRqHCQLGcZOUKBVMgXRH3yr%2FAwhlEAAaDDYzNzQyMzE4MzgwNSIME9aBiHrsW5VnTzC7KtwDba28hA4LjD1JLFrGswDeMZ9E5%2BrPqT0b4mko6SgFtTQVbhGoYDgRCfMtGUfA93Pk1CWE2nolHJou1a4LV5vFQRn%2B38slpUDLxY8TP2vPDV76gOWCTFuAqIsC6WaStEW9mdgrbrFlqJJcdorFZC%2B5qNHWct3YQYhBSiIXs33AbaGmmBpA1tO9JISpyIpPb0MUPAr7n21SHn9R%2BGk7f2DNs%2Bu816%2B5iqAqQAgkmF0rsBLKarmEk9UR%2FcU3wT1i4pk69LsIMlqzxEv1rcjqi3Pt7Rw0EgLMfD%2FM%2FSHwmmljakRcMTvfatuKsdZArV7TScdcrmN9FFhfUBg54EUQ98HL1pgoibDC6Jo7PVB59YnzBzOpCEKfQOdnlZqnwMzqj5XE5NXxZrMyxjJpuHGA2BP8RTqHZIUz6O9OkGbu5ECv%2Bujjt48ET8OCUjM8fusp9e0hl0DE2v8v5M0Cdy6JjwlkELNOxXVjnkA1l6JXfs7UHjmc0Cfv7MSoqQ739o9eXowkjTmxuGcdyyL8smpvs8oKAb0dH2Ekih8c%2FcYtwlBJozM%2BGYrcTERs6wR6%2FUNNzgCPylij3o%2FljpwR4N5TMlkJTJ1Ji7hvQqjPJP27QLNL0ASzGpAG5KpiULBpOfowqLjvxwY6pgE7fRAsqRWEo2iSA6CaJ8NDAXsoN%2BKYsuhhxLUhdUPDZelVbh6%2FZ5P2jLMojh4ZHrbszseZ1bF79bPU5%2BgMySDo0dlSf7E9qgQLnLHWwWnv7n7otqEloW8NeJvVJEsQBAEp96yc8bbVG%2FyUmNehcrLMEXQEPqc3pa2MPDPEnWTV9pJ2bneUFPvwJVNvnnw%2Bs3%2FFeLk7Z4keEfQg80AN8J678E5ewvuy&X-Amz-Signature=408cdce6ce3913aa243639342af995789c53bdef803a144467f340f74a9eba68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

