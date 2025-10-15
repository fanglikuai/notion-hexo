---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CL6VZYZ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICRe%2BHRFklbaHxIfnGdJCGxh0NUWI8Y4QPJHIvxV8a6EAiEAnSD%2BUc9auSfwUSqYDBaClDTwkrXpuhZGluAHrt1rnKIq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDLAhI9pY31hmgPJy4ircA3aaVI9VkvLsiPY%2B3MK0h%2Bt%2B%2BONunnDL2tC%2BMoj0RIHZgJVHw8WeZsTgi5ifKE1yAToVz6%2BvKIL3yficvlEN0xRN8VA2WTVN%2FRNaa59u4TxvLM5BlSdM2GvzpSTSCL7Wd17vFgp9tHuPsUaYvlAsxC56MLWmtzDhApTUWfe4MnAOsET1WbJaPDNjuMpIjowv9YYB1vhXoONUPgfJf5gEj42qn5phHcWlLDxrNCAdP9C0MBjJUo5RmmCmJ7Dv8ITzGcfvdakaudtNklmv0kqkFrdb%2Bsl6JheXFlTbYdaCW%2BA%2Bpjvj6tHlXcq6%2Bj9KYx52lX7WJFan135BIi6EyMSnJlzBE%2FDNX3CSpXAswZj7KTkHWInaq%2BtscW1R3FBbo7wUVhcI1oNZFxWKX3ACJSTmkkbvm2qwuTAKHIEuh2WiE%2FRQkE%2Bupc9OeBzJOcUbBojFh1Giz%2BjI65jCdA2kwwZWWiVK0yDj9K%2BvBZE9mymmJNa8UCEzZVLHoiF%2BfOGrjmLehzre%2B%2B2%2B1iHuWFklAMSGtvc%2Bs2wMVF0eseSwWXWP1pPSpiPgqKHWkRKpPvMsK81KhIjet1LGE0uae3VqxXw9iuXChZaYaxND7313OXXlJF5PwlvdBmQ6IPpMNYZLMPOEv8cGOqUBwpHrYDnPDGv61%2BobvSOQ49I%2FVtKKkHUNAKkbKVKTQb7cX7MLNTqgWdE3RO7T60bo8%2BThGuyEhDIxeWG%2B4DOersRkS09hL41dqQxvpvON0J0u6%2FScCycaxGC5tb3chdoWBvxb5uIzT%2FltuGUdmndGB%2BTfF5N5eEKfYLVOMfiitT37nzo7mE3G7AkM6VO%2Fz518zSNfgLYM4snCdz%2FZchdHJc3URmGz&X-Amz-Signature=b90223ab59eaaf6a712de0de4bdfc7f6c34b448eb7a8281edf4139c22cdcd5bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

