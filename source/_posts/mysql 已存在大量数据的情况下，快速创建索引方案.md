---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU7IWCAG%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T080723Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDZh9HLEX2ksSHnFshVtidMn4phb%2FVcVDIDDiWA3WAwcQIhAJYkYmx8KAz%2BBJA0IR53u5NC8XQKdrDaoqL%2BKl%2FhFMzEKv8DCG8QABoMNjM3NDIzMTgzODA1IgwDTwb8PILZ6oPK5foq3AMIBRoqMqWfbq4mfeGeB2iNsZAQ5jTuZAkAD5GZZ%2FJN%2FTBFWgIhgrbbRXIOIb%2BQRzQsabc81wzVTFx%2BOqeGOTwyDcGmQEG9b%2F6FpB3al6K7Yj13fZbRMa311YeAYmcLvWUx3zr4D1HTDCoy%2FUslzrmu5YN7WKZaDySu6q%2BZsyYwneM4SAUk31dKx7TU%2Bx9B3%2FaOQGFq1x73Nat49HQahHdlXlB8iBp0Wzecv%2BXlCCEn2Ra1bFepXTPozBC1S9X0038IG8ZZlzzjfIblvKbebdxlVHVpI3ALDcDo6LaGwe%2Bd4pcRlsbIXOvhBvJl%2B9xQK04ioHeDQx3QUegCgjETBT03m%2Bvs%2FPdMXEPLKfvoXEp4Ez2ZY27bZLY5JCCh%2BP1BYy1YZQJ0CS8XDAXFwTlXDRdhrYx8pUxK8eAQwhUf%2FUj8%2BUNtAS%2By6LHZJX%2F3EFZ1tmMcTXt5T25kmm0v96pVQ5mAew9AwPhUQdOGjQrcOczfH6X%2BTnYn5VE7SH3cQQnTLwTeZvHvOdmJLs8hFybfMBEQpBxB6%2B2x8uToRwELgZ2wvc2vKVn1Q%2F7ymRJOhE2C85aYqz%2B48BiW4dau0ORPMgX%2Fyy5aI0SZ%2FESIS5V6FRVGxPdSJsftkIsSuGFtYTDc357GBjqkAZ3yvqVQyIeguvtSKlWEPiop%2F0p1205qMD4frxbdpiDZNyu2X2Pbar8n1BU5rj7Ag4q8ExrGfiW0IlZVfgqtEDfiCv5osqfYHHzJ2Xrc8XuM1sRKDgrFf1sYbFh6MReTvM3OHt5w9zS6jvWvBxmhXPGCKWiq1QckpLtwN8pcOnVevGM%2BtvgFXz5Mu31yw3p%2FhvGBE7YMQ2FK0%2B4xvMeX6hOW1veK&X-Amz-Signature=efa76cafbf7dbb80f7511828ab1f403364ff36c9be3273934a4e7d5b4532ca42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `<font style="color:rgb(77, 77, 77);">create table text_assets like network_assets_blend</font>`
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

