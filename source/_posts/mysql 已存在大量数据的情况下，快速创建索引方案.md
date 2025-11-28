---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZ6GNMHS%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcwlOAcG%2B3tKoYpfC24m5ghdAqzRIh%2BDNQA%2Bt4%2Bf65uAiEAiUZtRcqzzJzLHo7sq2JSj9cB4XruLfjt%2FUa3eKSuxScqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHXIwuJ1X9mx2b1OsCrcA08CYn3QmkvwcxVGG8aTKpHA2dvPW35anIeNvMPH%2FIEc5aNTMhxpeH51ziGn8Eo%2FM9q2JlttbRyD%2BxwxA%2BsPi8J6ulS1M9F7FnXKcRh%2Fa7P%2FndkrUBwRbx7eT2iNhnJ4TTXNMty50ha8GO3GyIA916QqBJiUnBwXlFaxqV%2BUuQ4FBjk5UJ2nwdFXhC%2BbXbC4l3YV0UkUTmdKvgKs3km5b0fox88gnpSFTD%2FgP0QkQUYJMygycCgaOHZbpGEnsPI6GqTcqZXAfC9xn1kZwepGrfJOaz9TxCEazGx%2B3IeOj9Yt%2B%2BG8ECThaw74XcjDOD1WH5dzBUB67P5WULX%2BrVGjpXxV7TlSZpYjU4UyoPiIpJ2%2Fcrz17s4h3DjwXNFdFOu1hNMTHOHxik0mzQX%2BdDaDLdQB2c4Snmg1Q4EO%2B8XL8n0rnZLTOWQV4vgwlYEzrHw8JTz2GWDPPF%2BalFTQzt2ppMCydT3ZbmK9P0sqbJnOrY%2FO3dWCuDxp0vPk7fHUrksQs89ZhlQk5em8cu%2FGHnRFmpiR3SZWx2MdNeNHfnFdhxrERtYvOSphA8XWi2gGz9vHDHT6hxDcNLh2iJPVP1X7QZKIQaMMit%2FJbk1CSJLn2XI8P%2BYSaxzUGZZiylwlMNO9pMkGOqUBAbH6NfrxE47yMlGT2JOL1wrcNX%2FytoOz%2FXYHV9asol5OA6TqtAU7yvuyx%2B2lPBkHUb4H%2BWXEGV4c1lCJEKHK0Es7AYyS%2Bn2AVqmIzJ%2Bv2qzIU8TgOVUxeLLm0JNeWwSm59C%2FAc4W2mMCjCDDlsggtcMcwql8Q9uZNHPSNm11ea2gKgF8xi6FH%2FEc1uq4uTJdef0YpRSC5k%2BPmCo3ATPxdHFCjC8%2B&X-Amz-Signature=584b3ee2063fc91bf162900cbb6a17fedd5c68414b5d71f6901dceee1787ffa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

