---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PNG6Z3F%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDfHpqbKyCJ%2F9CD1MJZ7SDM1o%2BdP3O%2FZj097Z9x%2BLPzGAiEA2W9UuPB7XxovLrNtF8PEqZBYwqm146q4sB86IrFNAQIqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOpwCNnqTjxEsisDfCrcA8F74QnDCLQ0ZTqU%2F%2F21p4ICkpFWORXDihJ0ja9SQh9BKInL%2F1PyvcyQxOszzfHbMCQofBXI3DrAaGfJ%2FkqI1q7m2eiYakPtYJPKXunc57P1hV4aohNGRq8CMQWlY3kt2%2Fd0ckSlpF%2Fk%2FefEaul98vJQp55YBuRNMhVwYs7aVUs0nX3BXfiCiq53X50uqGqykWtwKj1ekhefXmQ5%2FyKvYtuePW%2B5CwLxPm3eZxmgDkahFT0Ks676ABp5fMRYaasmLoypcPzZvMIDpYcdW6QYbGPS2F9l7HXeMmlJTPsQjKN%2F7sDy9eR3C3R2QmVOdewfKtLNi9nBRwyMLFwu0uZ2lxOE0Bt0lytABvunNVZAaCiBlbAkLbYBD2S%2B0Owz%2B9ITowVQXGAUaI5rDa4ZQ8hqAf31ZKvGjEPZjx%2BuZ4HCxV9bsurp2whXs%2BYRN%2BZGahOpOxeFYp5ult7PqdZ2bBe0jdoSehr0PbwuyW8hDvGI6dyGIDE1isWVkR8utiFPBOoitysGNIWkZP0nLvBw8xjw6pSUt2P%2B8nAkAnHp7S3tUo20ZbpQ3Wjdnv9eZSFu5aBLCrwRXh7fl0qAbjIIulxgxv%2FnFVBFJBJPG04ksCGPr9TPpDp9gysTAWdM9qzDMMbBxscGOqUBGUAlKTewJ%2BivNwUdTPLU2LI6ZmgCPadCGyz6PSKfLLOPyTTSYti9pT8l2eUSDsVJOXC5Ziv7BWs3CmIJkE2cQtAntaqVadQicLTxcfyoXAD70%2FRCBmAuAy3ySTJANsh2Y5eKV9kxZZZpjdE2ekFqgLWtZoF2IOIEaHE4%2FCBJpl7IV5BA8Er9dHnQ7Ioj%2B%2F7h3ixXKuI8i2jqgSdE%2FMHXxbJ6UMS%2B&X-Amz-Signature=5a997259df1aad184ce1bbd2bd05bc9f86082d64c0f043baf5c141fe9954ff82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

