---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663L6PIL4I%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCC1haMb8PSYZy7zVmRu3PdZlNmuofpwAYk2vI7C1zLNgIhALu6jOu80jMCIFhHDLBETJsaur4nePtIMefJFJFOZATeKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDyCHanGKcPeQ2g60q3AOgatAj%2FEQSOVDxSdEwE3heet7%2FBQ82P%2FW1PzCzBrjDERTzWtY87CKci0Lx7LHQPKurLf7Y6lIITVlS%2FUpNUhjGbAwYhIe%2FUvr83oO7N3d8is%2FxPA2e%2FWxuIJY7GXYo56YXLs8Gj%2BqXNbYD6x0EKWiu%2BKyilRtu%2F5yx7aIvl%2FuGf04k8WTHJsn4lmXqPGg%2FWj5%2F%2FtTnJjjbK1E1Wc5McFjfuuzqiG09XJ6DNKe1B08cMzIiiKt1ChaduKCwWdQfT8ElnNVWJJWVbzGGym7Q2uFkm4rM%2BDL13weuRqE5DT6qacry23rGlBdKd2S0Z9QxesJ5qjm7qxgEqTwhA8xNWdORYb0nZpJ1FvH5ilFEn7HAdrv4agRq19AfAFFbLlmleay76Juuk8FxK%2BHX1vV2b4dQsPT9l4hOWFbpBt1%2FYOsjZIzgcvQBby36ae0oZw%2BN2f9L7l91GCWAJaDJLpOqBcgUDvgeFu2KQHds%2B51%2Fy7WPG1xhr4ojNrZWrjckaUEGxyX6ANYAg%2FRsoWPUcrnP1gwfYf4YA%2F95FqYZvWj0EYJ46xA6xlwvJFifDJdYJ2PHppRcb37bQKDxMHz%2BRCRVXEGFTFAHJnhNJg0PG6PFbLXwlUH1VOtxvcbfZwu6ajDgsI7HBjqkAaIksmO27wEPUQa8fgXL1gyM91hKJ8uKRddobYZsGZYhmRJktdLw7GMMch6fsgMaitHDJgiElQaRJ6P%2B6cuxzRXgGE%2Fyb15hTictBfJWyaqiBX%2BARUDiDSkSxcofIy%2BL3wp0iAsfCoNhRjeKuRsChDpEzWhd7VnZ8%2BRx%2BQRzOoGm2MYt2RZ0c98bzYbs2AJaawTirsj2a4lu95uGAtAGhcAcUkY3&X-Amz-Signature=84707cf52510a2bf8e104a3f23e91ae120f3b85b182678053d355b7ea94f25bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

