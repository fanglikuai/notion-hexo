---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBBRLUA%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJGMEQCIEjBHL2LsBznpLS0Es%2BmWbCDUIv2bRwgqFzCeNzPFe7GAiAxFGA3SSY331n04TSNE6A2gKHdM5HVIpwS66QnH8mFTyr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMBR%2BfTjmTGWWJRCToKtwDiXvJImEHX5PMN7xeaq9r9qCadzVIPEm7IICk1z9C4%2FBotKFoNo3ss7Q1%2F%2FoU34PTRVCvKyPsI%2FN3Go6kFtsAmYac0D5xut%2BrmRwm5oTn%2FhVDkP6ojjSKU0IEZn2upIpL9fbbIThiND%2FwCma5xAdMAgljcBdyZi6fC%2Fu6DCq4xpy6LiACEFHGVfrEU6Tql3QDd7Q%2FymWS5P%2FfoLPZNKvmjxmPVpeLdNnaXBmD3MTaDMR2FafN6xe1l5NvTOy4DKCdcVaOj9J1paPKtNRHD6E1acMyfZNmyVLRKeV%2BWONmbTSA6w9max8eobZw0w3OguwKYlZtJ0iQwlIbSKgMlxhbvu71ntfgppzrzZGDhXhjUZZp%2BRdKEzpL0NVERQAV0KFmNqFW6kZjqomAycRx7kg7f9cnKv%2FXMyMb5psecYnVOuZh52ATtWVcHmR278XmkIZzxmJ%2F1SJzPs92TU25EQk2jz4RDweIZNe%2Bs%2Bd96%2Fr8OCS7NFMtbNv49ILmw9EW5Ys6DklipTHMMSbdSZqhgIo%2BazrG1NohPsZwzo3v2FwwozE40kswNYjmdBdmMWM0CYfW3S12quIfqMOXUaC4Y5pgvT8VlxujNi3ib156fX1xBaRZ4pd9CL7zN%2BkthZgwq6vPyAY6pgEF1YbEsSnd99oxVmZ6mJI01EydDnX4TYToG4ZsGmvbrRzilzr%2Floiftdm1tPx6JCoOtBCJN0l7Qpb7HvmfdSdrrQWCfn%2BeXS2jxnEawe4C%2B6VcQqjs5dXdZxA3%2FuZZeEt1mL4dtg5UscqLEOEItVpM1p2osaTvlUvKRh7GSOUZAptvpeIkYnuoYAnJ7vsMqm0y7dRqljX2vk7%2FSWy%2FYm%2B9TdXxPnrU&X-Amz-Signature=a58de0da519644a96a230511a0048ca2f42d2947692a9f963900c42db658bceb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

