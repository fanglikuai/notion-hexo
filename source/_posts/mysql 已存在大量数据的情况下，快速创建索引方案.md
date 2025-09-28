---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJL43OOZ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCID8U3af0iJIuR3kk2VFg2grw%2B3i5LmBWIw89VN%2FZ1iWGAiBJGabjtkovF5FpX10dcSeBZ6Qkt4MgjdDMBKy6KWxRnCqIBAjG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNlUH3tByurJUzaf1KtwD5RupOFiYnfe%2BEIiJf3Nt2ZPTGIVBKWKi7uDyiuDaqAWVgR72ITpnUX8aaAw%2FcwTxv8F9ePvspAy7nUHWylhm%2B1q0faHChR5Lal5FvROy6hKj1S3x7k0wmIUP3YIuvMv2tno4dIKjEM40hDLA5xr0U5EgxmogHYK9uy3Ad%2FHuIPbjwbXODVUYrGybPKd%2FMwb5C0WL4oBFCHYHPubcDjisrggCnib0P6ASAe47B1Xva9BdAebLriSxcg1aNzxiwq4RJrZH2L1vAHvw%2FML6oW%2B5xZFSSKk%2BYiBHKu2BOA0EKtmV5ZJ%2Bq29xn5oYtEdlFOgprp4NEAB87jhNeGKuNzJiQdh%2FgUDNyRjAmiAyHW4yNNYfMExkxqJJu5wrDU5GxI4ahEc2fnMW6wbgiArdl98pg2bRRH5hvpJkmgtZnG6ygJe3xsRBwvh3svgwrZcqNXFf4r4xdhpTkgYu7byfbCOh2AR%2FHfk6aJ5n97xfbteqRYcp3pCmA09wRZ1rVeLakJqhpFhpAqup3zOPtc%2FgBMkpkHS4ZRNbmB0g%2BUXgSJo8byUxbITOyDhfY%2Fudzs7VdAeA2PtazchA1AkFSVsH3nVXzOZEh3PNiSMuza3fCyZzsU5pllTmk1BQyEtf9QAw%2FbnmxgY6pgG1tWIMsXuP80n%2BlIZlZBBycX2Z68RIbhrZA1WS4bgjHTucMK3J8REwFGG%2FKUWeFzaLmPJoySMzVt5QyrmNh426Bg593lWju84fx%2F2qdJYBL6FVvB8BLLb634hAnrL3R41uBnBT4NHb%2B8bB%2BR7SFipn7s%2FGKJ1R%2FU7UYJUZcK0CXRDipJAOG33g1%2FTJXceyLc9%2F58stWJJsj19ebHxO2t8XGHb6aQ0O&X-Amz-Signature=d60feb99955ffe05b8542f109c76cb0a07e48c8097e9f0df74e20e00f4f265b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

