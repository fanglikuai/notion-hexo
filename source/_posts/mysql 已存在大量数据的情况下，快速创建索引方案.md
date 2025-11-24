---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XK7BD64H%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5gPTORWipV7vOqa4k9%2BofArT7jMM3IEkR6woyxKcXhwIhAO9kAIuL2eWAawFjnnaCJTFZoq%2Fjjih9gOyjovbesnBnKv8DCFAQABoMNjM3NDIzMTgzODA1Igw93r7MGCpqm9yR5lcq3AOxpIv37UUGhrirkvSbG9zmLstyifO2Th7AsoRDKzO86OsX3PIZvU8AM4x9F1MHaNYEZY3spb26x6Lk7UyX%2BXituCpTz2LRtvKG2AIl1Hohf07v1evg1tfW2DLlIyz4DeSLNP24wZeG6ovJdIqjqd66gmV06CIW1r1nMzZQUsEYMJNzcGc%2B898MgRHnghyeA3B09Vhiq9WUpVewLGnCKe6YJ0JLy0suY%2B466jNWEEk5hS3NvtpElz4SzOx19K5U9lfO9xkd6NxutsjLeVSBXxJcVRgfF7AHisq0N%2FUL1wxpPMimRp0tgysVR395VOSdK12hNDE4r3s0TDNgaIxa4nJkNP4T%2ByEpJ9PI3FMRTPWhKMqPsYuAyHnkSH%2FKDD5y%2FXi01RnEuYtezaMDGHwepuPrGKWVmCGUJRnkelX2N6CnQqOKOS7UCeHj194uVTqDEv6ztpc%2BP4lNqNBQXAWCEnxCpoXWIi3G%2Ft%2BYSJstuw25orzu0UEpFbfQea%2FE3eysBHKtn4zqp1OaGrKtvVP577hes7CnhGLSLwyUBaZY%2FHoAkgjkJc%2FEsjxtGGFTNdDUmUM%2BB7UDgmWPoDsS0fu1qHkIJoHQ8NKLebNaid9DZMlilvc7SFQq%2B0TePEOYPDDxhJDJBjqkAYBM2AJRweHUnnjPP3dBGvsvEGAw6o0z758lxNGUmoSUSrh0jYSlpJXz00uasId5hqdq1GYDHjy3wVhjUqqtsmsujFTIbD5auMBi6kuF49RzfPgdm4D3FACvKscfk3LrW48%2B07Dao4AscnnXSztXRtBiQdkIq3tBwjbluDH3w3UgeQch%2BQuiXuSz3wq3mQl8SVWh8d702urJHF55YBlpSamiDWvp&X-Amz-Signature=5d9a2b1137ff29cbc7ef1e33e77387984ab977eca60b3f59556afdfcdee83e42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

