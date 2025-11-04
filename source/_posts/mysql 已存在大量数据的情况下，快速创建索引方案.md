---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUEJP525%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIACWTek9%2BjOuX2CgoZtXSLqSg%2Fl5DtJNzCM4z6c3D9w0AiA6j3KgUWYCCQlssDCtdTNG8yuOGTH0yRGPzrfKO8ZzPir%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMndUqGPujPjgATe4vKtwDvLClWVzgw7tU0sD3Cs9NQE9zUEjPEH7h2riWXmP%2B4%2FOzpcG4yMTEHcNA%2FuSBKFcaFeZTl3I37Ljwo%2B3LHq33tz2zSvv14%2F1jrweQUxrk5NJpPc2ra4FC2ASecDM2xbeQ%2FyAJN46PzFSwSkJyFkOfvKnAwo7QuSn2aXsuW8aCR%2BWFjnRBmikhwp7cl8%2FpTtC0f4UcmBlrU2IJGuDLuyias2GBqrMZxG5riBEEz4UAD6EmI2%2FGk9It5DaW4jqokenS%2F8OZOZsZzBjBvau%2FOA2%2BxArY1QCRA2kTvfvcVTg9orpbspgKjTnHZuQef3Z9DiiIP6Y92eZb7%2FsXMGPvr6fsvY8jVLH8Vl6W4k5cWX%2BwG0tHsGPy%2Fwsz8haRady1TTdqjtOWvM9trWGwVD5vJi83RRxgfoj%2BA5ThWWz%2BHa5YOJ5UHsyX71onrJhC7ATbDHwjADinsDHgMVF4DD62XJy%2BNDFPb8t37OasWR1gqlqy0C20PVUIg1P0w1MVQfcltrP9zldiejia5DGeFhNEyBfuQakfv0MFSeO0aXjZFZ4APFFVOnSj3ou5wzuIFcQVD%2F8hqfYzRm2yJyVofW2k%2FGdaelEISWnEt7ObdvxvRhCuRePq0JF2sjQRjIDUESQwwLmoyAY6pgFJz%2BTp1CkZVO8JTTyXg6TwVOQ5ZG4AfCWk3YGNLVTQf8907c%2BnSbVGIcar9Ab5KBIjv%2F7xidALWXfN3CUkzGkOCUv%2BoWZQPvDtezO4sA1kIhqHYgYzy7wxmu1EoU2wbVF%2Bom75YyXYp3Vp6w4vdIRSl1AwrkM08v1o0eSu5wa0wTdpTcqMGJy9lQzAwtqIKXD3zTxED2iZyGkMJ%2B%2FCdwbSSHkCvrda&X-Amz-Signature=3ad1eb84f44d40063aa633b2c3dcc87d5be2398688e19378ecaa83713b012cb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

