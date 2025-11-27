---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDKPSLII%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICzwbPfHJMbgexgiBJyFvrtdAeadPlvmP8sQYfeWjUaVAiBfg17RwMEgZvEr%2FZHq7NxAyRRfOQ9ObsugsMg7E07QiCqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjkk42f8LP18mC5fYKtwDhtsYSe5%2BI7777XQMRQPQJ4UaTYLDV6DvuNvHnYarROuGe2uI2Elk%2F%2Bq4smb3NatSq8mRfvKwmfwk0BTFSjygyT4lGf1IRETXwmI899m5K8Y7vcFFRpnt%2BGCRdu5hz6%2FaM8p1E9KamjU1xVyKiRIgG1nakEuPPJIQkEzBdX%2F3PsU2fIE1omTtFw%2FyZtwcLvXlhBB5vM%2BELJH5%2BWOBqFUdgUMASx7AJgI301IsO4YhwFemBzG7bzW3QrfGhOTacjr2HIQFJrecYLf7VR2MzY0tXhyISYaJHytoiyjs%2FvsGHY0iCiKQk4lD9iTtqIPucjAkAouUD%2FQTelWR%2FKp3xi7GaZZjBQH8PZ9VHJM%2BW%2FdqHDeqo75%2FWS1K9VBN4tJRR5%2BcTxfxUEfPvWQHJaX1vsgWNrHMfIIGHwEiVSks7tU0C68GhwAwBgwP1ouH8Y9u1XI0ci1cEAIVxqbArKtguYHUVKgfcWrU3i3AJkYEvdHO1AlFZ8MIPfMj0HAtxciKNGbWXng03mHz%2BmK7Qi7HD4SURXij%2BVSiIA4CaLxZ0RBa1abSlIeiYCWfRZsiQMW8azcv%2B8fSpqhjd5U%2BkQ9I0vujSiJjawF9lEHwGTEl1lHqOHOfJDGHfmtTqSNar2UwkLmeyQY6pgGsaVZ0C%2B8XdwDV%2BF4qNcu1fVwd5YzAl5130NefHEeKCf57CHHo0onNv95vXyFTzBS3ueTCQBwwb%2BVfkQ%2FlBNFury6QXkmD0t5ECbWSrtcM26HJxE5M39PfyhnfMO8z54MdySKkG8sd0jLpAPBRB67oKKNsio4aULO%2BnrYZa489%2BUe%2BguPstyfTNXkQxCGT9LVO9VmTbxhDP3IK%2BGRG7LZQVGeitzG7&X-Amz-Signature=edcad820f77880d26280fd386f49a606255b14ea10ac53309e514f9351820525&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

