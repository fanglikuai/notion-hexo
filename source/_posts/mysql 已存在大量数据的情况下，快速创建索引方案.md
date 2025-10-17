---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DNRWTDB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH8VwN644h6ht9u8r6pEkS3QCOGA5r7WUVFDRG6TlgDVAiEAqtELuQLQVbxOKUCxQf1SaNXfk1v%2F9yKoBnnJVm67Q6UqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJD5mb%2Brk2AMMPw59CrcA1KOFnvu4y9sUi%2BUXSLCIAdJblUyLbdzRgN0JvbkGyRczTXNeT4ifEh07Wbz4I0%2Bd%2F3ebA6kghM0tTcI%2FW3Im4iNY6HY20sL1TBLnEdhuHwQtG0D%2BD9gisloh3QKJJNcVCJwzRPpUAhyv%2F03Wd6q95Zc9DjKZ5vPoaBszY%2BXY74VNfZOw9undw4xghtr5SbHJnW5UESLn1Hc3Fg0GTk34n1IU3OxkG7tnJugJyZkGaspTl%2B5Nm%2Fsig2SgdZGTw0W4PmNwhcvKUMHmpFqZO3oUskHJGZvFUmfAO09AhHP9reqK1gBFf9TikrLjCsjoW%2F8%2FRrOYd46rFWuphnzYrrepNeL12WO%2BvXUwR0JxV3LvLv6gbUQGXU1JdxeM0zKy%2BXrqqOBFVbrlLjsusDI6jMQI%2Bx8o3PSqFcrsJc6goroQp7ZnMdwHMuadbNOXOYc3rgX84C951wOHg6L3Md6J3i%2FNWmhwnwiPCCj8S%2Blav8Lzs7Dq8sZt8g3BkMxtUka8oM00%2FDtYy%2Fu8vL%2Bc8IVmRnA4QO1KqBhfUAJZIDEtq4gvXy7Canf7mzNXZfdoszdaPexjPY373vywxBxrE6iR5YyN%2BnrMEJXwedVBgfzTcCdeCAykY%2B6vszwMD8nL0MDMNPBxscGOqUBQLBxKq6q9WipbdTjOKLDXRWY8i%2By6akSd3%2B6FOUiG8%2Fe1dxPpqt49flNo%2BbV%2B%2FVADunV76JSzbR2szvRkEmC%2B7pxdZRIF6lisdeOzIWe7AnTrrRlyoIEWL8guF0bbplX8iEfA4EXauyE%2Fw8xmbwdqT9lH6BWks7cxIYSgP1fy%2Bp4TApS93NiCK%2F2P4NglzqlF%2B3pLRLgtYeOIDY7cy7J1VYe3G2V&X-Amz-Signature=528a2c987d1c6a2b7d02051987c19da774eff628769b4a46951c50b3f782c4e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

