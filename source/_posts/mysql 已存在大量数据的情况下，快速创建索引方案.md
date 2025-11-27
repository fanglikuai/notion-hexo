---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WVPC7KLB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9zGmgE5LUUTy2pbWisTFJzEJ7HZD%2FbRNPkcBHjDLChQIgUEsAet5%2BIxtFBPBq%2FcwHhWCqRgWjHI%2FwO9KXFMD9D98qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNmBSSWj0a59XupbkCrcAwjTFvJJOwhB%2FRZ18i0Sl9Jc1j%2FaFY5o%2F9ifiS4eOwG5jVOIV2Xpc3LPwqEGTO28THWCaWq7e73TxN7MpgnhT6KVlH%2F3%2FgGjlRqfGUdAIWhVSB%2BBgCRvZJ4490KdXDtxhFAo8T2yLrx5AY9tqutRkySrmJxea6vwy%2BKU1MGCLIwez6zrVYQ8S2UGUPD%2FmwwP5hhXLDQEqQgL2bC6zOWgo3uu55H8D3N9sZMCns%2FMJGGqjP6vbLcVMBaiiARJE3JXv4rDw%2BaNF%2FBIm05W1gwSMKPzU2o1ch0%2FuvhS8J4l%2BwnAJZjssiQOyfP5WTFezYkmg7hmX4ZXXCUHhBlzCZI5L%2BA%2F4DZpfrfJTiVmAM0yGKIXABccgfHEVZ%2FIb2tZ%2Bvkf7yqHfneLtIQxIPp3zMaVKwscTZ6FamZxvseXXWNjrOhXHKOYrdA6fw5i8dtmoPor9MlrXIartULufrKkDqJz%2Bym5Nz%2BY3w7rs4FNvbje4b6VaDyxicmTRRU9z%2B9ALM0LoOLw71CiN%2BDv1c2qxjKlfk7oNnYK4jJpIsmaB8S5tE9lKsVj1w9Pi5FLjYjH8Hg%2FKJr5xU6wDzJazR4VqeM72XM4ddFcMkvAOXJe7OljXKHMJjloCd5X5BmHKcxbMPXEoskGOqUBfGymfTUBqc0JF%2FbpvQU8XixXex%2FQ9ytskO2vdDwJtnphYuN%2BwtR6scLDOQt%2F74GFcB%2Bj4wu5G4H9YOqpZebguzsoiNL5kZDZiTGPWLuVPmSbTQKypNjKNLtfspx0NFZuYfH5xL4m5DN5YzgS5AhKAVKqGKfky9MfO10ZvV1hwWlLl70MgiL5iZIkVN8B4EZkSRm4pOiQwOa%2BzcwCiCczrMktdL2L&X-Amz-Signature=4733337d50a6b1ff077a9b39015b480a12a0115e82a258b6b33cec4ecd60b442&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

