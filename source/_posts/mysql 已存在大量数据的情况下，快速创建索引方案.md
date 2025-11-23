---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUNMV3G%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIFevjg%2F7M1i3Q5niqZS6NZSaAkRHs3qYJvtjPWSGXJRLAiEAklhFipRIFIrMF6uqGEoNjMSkub6mLaPk%2FWx3kcinWv0q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDpzPmDdXWGCRGEeXCrcAyRIejwlwUhQoANlEs21AV1Whqlw%2FgE%2B7SW8n1r9LysE6uK%2FJ4KZg1WFC%2BV%2F27MOo8yPO45VLaotoa%2FiHql37edL6xJzVxR5Ok0xGOH2JJk2Dxwn3b6EMZx2lA271kscaghQs%2B4TcpTorlLhudUwSHHFAAqxmKKB823YHJ7RuLn2jPWkkybuZF4sRq74uzlKrMqP6GNTJXTLD9vfKA%2FZK4UgC%2FXc0nBU2ZTGU9preiDNekMsuaWOYY2VgPlRwHd841qMl3MQ9FymXPhNiQLdtBOyl5RNuO9PloxHQsnCWJpxdO%2FKh3x%2BvHgFYIXwdQB4bO0KJ6VI%2FMa9IRr%2Flwlh5afBgqyd6uo%2FSeOr5BcDMRXeGgqIII5lBWcZdZrh%2BkTEU0bEv%2FgYlJH1k%2BkMr1hjhXvnsAKsl47%2F5ybAlJflWt5AYiE2lVCdGxwOBjSNs8ZTuijM0QAKKi47jmQ3Xw9BxETaDv2BKTHzoq11yE7Nc%2B7FwWAtxyynP%2FBBc%2Fp4DCqirHygASTDLc2HiIosmZUsajfAr3zvTViDZHn7EA4b8ZuIggqslCmS9LvB2mpdHc%2BJj7AP1p76SYTEzFe8RK%2FKcQB0aMnETGDyd%2F6sxLaUhd3Azh48ja7smyvgr%2BNgMLSPiskGOqUBGydC9RPiYlGNn4hUUaNw7u7ieq0HpGhEX18GX0u7rLinA2PWCb3gF9vKBUuxVKKpdTVYjsRrUpY9B%2B2QaOWlemPSY6g6DwiAUc25pp7OrvqS0R3G7TJqjzWsj3aL%2BupIFO3D0bTTyvqpa0ajwNAyUIXkp0RkJWAuvEI%2FfyPw8kAVFKfX0aa%2F8HFl49ViqWB1ylJBAbAK%2Fp74mJLJaisVsjcmcWSf&X-Amz-Signature=4211d680072d026050c7bf2e2d5476fd7a1f7ac1ea6de9bc20f975676d2124b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

