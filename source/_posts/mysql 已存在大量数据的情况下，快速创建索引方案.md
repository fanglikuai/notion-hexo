---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6F7ROUX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC1liHvDd8HCs9KqQvqSmFc0Ena2y2NENwQgKtN1bgapQIgeYYtNhWXfAMn%2B3NGoPJElwqWdBF2pG2QnKnIAhk44W4q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDC7dy7348baSzsMkNSrcA7C9u7CkQ02h4zEzAbOG4AS4fVXJMyvSrtZUck4%2BMmjUsKVWPPStPfmV2SOl%2B4CMI85NyROlrAcB1dCuXI6Bo7TU24BIlkVNrzR0FUjNRzYwzCt61xOTgJlBtGPhrpszh7AjRFExF%2BJ32d7q%2Bb6OsOtkiLztqA4S4biLNlndJwQGdMNe%2FqxNoynyW%2FugWx36m%2F7eIV%2BfzexLKGeC537jqX6wv4CvDdktgpsP68xnOFFInm7C5Hxn4o%2B%2B92gBhQbjq1uF1EKbBHg1A3yzqsSaLk1cfgNQbyeb2QvLEsSQ%2Ba%2F%2FlMKRplg5k%2FamGuATJol%2BR3v4pMnkhmhQS%2FDQ0s0ZHJowl%2BySlPsBY5teRApD9F3x%2BKWyVFKoPFMI409VdLgEAE8DQFDJlrVd7ly7Ev7xpoen%2BUSC8%2FEwL%2FBDNzcDBBiWxb7yFgTTpHXUVqELOSwO2D8bwEar%2FAxzjlwDC8F7Balf%2BRjNEZueEmT8ucjE0cvoOP7%2BGkUEp3uD1yENDVbsIsar4reOcbFerga5fSKLS4SBo5lGq%2B9SyCIgQ6tfrFu9hM6u6T58UdW7HNgjp0BUksnF7NDAg8LUMM9hFjtUAghK52tTM2T2pS3tNjHAA4aXsLI1rE%2Fu7W%2FZ6NJHMP%2FXkskGOqUBV%2Ft7olwhvjU%2Fg%2BG8tL0ceKzHsbiBCIQqDzSZgAFKwCCNDtZQnhOtkLsxqyeYQ%2FaGqDk0UkXvdf5bFGzQ88ioA5s6r82OGTg378TLxj6A%2FLOuM1XOYnASpuQYdd9O1UMnGLA0GbgQinSPfAivR5F4dfSDzj8G2n57Uv5ItJXDx2mdgx%2Fi4pkc1EUCFVrvHyXpSYZuMDPC0lgEXLAtf7pXI0el5FW2&X-Amz-Signature=a39702058baec5e3ba1c1958888d8076a4add56b1795a77e03086614d3d1f975&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

