---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLMZLRUF%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCEhsMQ0xbTeN3iHjLvJMs%2BRVBxdhCUvuIRjEr6QwUYawIhANK8VjEUM2wsRR31ygRPfeZFSwMW5oivxnq1NGxVwzQ7Kv8DCDEQABoMNjM3NDIzMTgzODA1Igx4p3k%2BLFg%2Bw6X3yNgq3APLBZoSaAYlwlIYWWR6pBABricNjKO%2BGD3KdHcc1xagi6hegivPE93UhKRM01Yt52VQF6VtE3AEaHzzEgFBBlRM6A3%2Bqle%2F%2FgqS3URVxHWa3OP0W8Gq7Zg%2FwTjCZ%2BOVssB04WiGE644wLvTqX%2FnqGo4%2FKeF75nTFQiGvyX0fp9jukv49FzFM83XC5YG5hI07pd%2FTDr3n5hUKr8hQ5c4SYdKXRXziIZ6LFYyCjVi1K2sQdZsguNXeCg3W%2FTjPMZGDe2HVKZ%2F6yyEoXvW4vpwI5mMlrqNDGh0iIxaFi%2BnDRleLIrUXTbBpqlzYQKROWNt7hLcer6wXgnjfoox5xuIClDBSH40qX4CzeoHAc%2FEWudu5w0kzBhNfXqo5RlHfbP52IqJYf6QGAowE0JOqIeIEFbXbrLNR4O2fd%2F%2Fm%2Fz7VTjp5nJT8fhHpZLrnxIE7UssVRRri1khxa%2BvaqnCyOaoMgr%2F%2BLFu9is1JqxKMcZzBmQ%2Bb92AqdvfWlsaAaHQOKHuu%2BeDkqzuzmReXvF9WoyvMWeMvfKuErEi%2FhpDfGDxoDpdTtkAC6GfzKhpEGpfMKit%2BRr%2FodeFm1Bxz8EwJ9I%2FyN0pIM1ex5lUSenD3NNXZ2kTo5YpdI%2BxQV5GC5VeYTC6n4nJBjqkAaAF7KyaigTdXQxe8S5PREPdV47fZFE8HyRLYUT6PiTPEo3MrifkpJcFK2AjVGCjyDBBAs%2FFl4Mx92rStgfOfKp%2B9xsEvQsTz7XBs%2BuoOTMuSZvSsg9Xtt5o571zMtY0wK8Gd%2FwGO%2F8a3ycBeayidjGINQtPa7xyaLSHALbOVRsjeKlVx2mf4%2FkLI%2FIaj7fbcMnLbicT0CxZRs%2F%2BDw2pv3mwt21C&X-Amz-Signature=86cd98aa2d56527368b8b1bc288e7532e6d2ff621bcee3428f7d5864c0054db6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

