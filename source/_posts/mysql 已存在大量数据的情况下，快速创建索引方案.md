---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32RHWJG%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDTATEOwraqGAmonyszGQsWK7OajEqZo3S43GPn65c6XQIhAPsGwkf8Pzlw0uWnTmzBav41cIf7YAu7fELsS6eGXZ59KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzoGpR6xWunrTi06rgq3AOYgp7WgXVwMa1qxZ%2BdrtVCxrGLiVswzTHFxi1XsPDT5FSp74pV0HSIw%2F6GJv4eh%2B9eYIJgqfCdBsIOLXo8Krd5SvRkhO9IaxTgot%2F%2FZUywhgycQiHSJoqHMxqgMG3bchbTe8FJBJX3QcIMPSVTfsEvBjeMj6rwPoO5bt3dWJL47laPveztY%2FnZAEyCA%2B6ck1El%2BZRd%2B8JBwhuwF4h3km4HYkaj3R%2BSA8uqi9NCk5EFBCv%2FigCHh2GTEAyYfDZab%2FbVVwyUZvYzp9VwRiTuYqucs%2Fu2k7w33mRtNBfxKMNKNd4Y%2B01MAUSll7CB9RzfHcrDnG3JIx8zjbaFFbEEiJONNA1KXyhpFsZJei2D4g8S%2BKEOvf7zRDoQYNvl0eo0VknLt2sGLHJu%2B9ccfZ81ZWH1LqkmIyCntXh92okV%2Bxe5YiVghbmKAJF1qwg2LAoHTd0mA3bH1Tg9x1fneB1uN0NdtYSERrdz%2F2b7sQGR%2B4iAHLWvJmiPnteJFP31lcjrEfHd5w8tSVqi02nj3pS0PnTCqwbnMy8Zk3J6Ox2t6jgw7JvoIkFD8VRYs8gXZnd4Ea60zc%2F8PTU5VsnDTWHGCojKbG8JUBj5Pk%2BOemb0apJoj03GrQNdcl49Y2UCejDN%2BNXHBjqkAX1lXLGGPr1rVfhgjuu6ACAPKFuaFsDL6YXrekGNT5a9Lzo1P4HvMSDz4fWlw3KK4EaQ6wjLl3astfVXlnqrZcD%2Ftmh%2B1I3KHR9k0tTohE8eLmY7nRcjl4XEKWBiG9iatoY44Ov0wSIHCbNYVpvkdb33f9E8waSW2nqIIoYj97kxRYLqbalMoYn2D355WIPVvcVNCOHMo7wFLmq2W3SzNntFOOFH&X-Amz-Signature=86ab05588a1e0b5653a3614b25947004d36fbb57441f4dac2a838ce2497a5667&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

