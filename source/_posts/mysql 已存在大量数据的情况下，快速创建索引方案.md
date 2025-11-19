---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JQ6V3Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCu%2FfQXvtkmZSpVYNVYYieACJiux1AGM%2BQkwMIS5ujOBwIhALPvP3I020sEUXxgy25L77PJDKsiXt9UqYI%2B3C9a0iKKKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxujie8dxHRIKYFuPYq3APIn8Qm%2FVyX4sWqSmAxu49QP%2F8bUWcWcegsaouFzGOMXLRKksof3YDB0gbegDNRJkmYlIi54Mj7hrIG09dFziQk%2B7IE%2B1VXNf01IXF1sWlfaLjnhxUpi4iqepsSwsBkNacJP0%2F81ECOicxaOp%2Fkn0EU%2B4Fed8lhkj%2FgoLMLsED3JoF6n5HEJ2mGEdjqPYRbIlZP2%2FlHAvMTAJocHXExh1gEn02TUCUt21%2FrliP7wOYWuFBqvGu4cWVa0Xdx6TKM77PlhNlbpdyVKcHK7vjzzy0Mb1%2FOXynW1XTa9NJtk%2FoTKbjTVeIhJD6LxdBNE%2FXj4uDbAvQPVij%2FCIb4W5RXfo2rQqblKUUrrGiNKYo6q%2FDQRTFNw7VYtUU7Ys79qBKdr9t9EFkFIUU0I%2B7siwOtc8bwt1H7vuSLp8TYec%2F0vNxxG5IhrW%2BnVnBZr37cgVcLNRK7e9gISJfiuaGEONZ6pfY12UclzJVIPTOX%2FFVfQRdLhuDL4ENrpimFSW2oFsD8vlHR3N480zA5dMtfvquRy6tXqmCvIBzWZ974io7YcI6Raq2Ojzq5X1zp86wj4ONNDtV2A6se7UU0PCWmbnlHKRQtRgNWw6%2Bso%2F4vfN6U8uAnJpv%2B1u4hkmRnQ3FURDCJuvjIBjqkAY%2B9fY%2Fca4k0LPlCNOCzAMY3qkQgT91xOpzG5xkcnysitMCpcj3%2BbmwTDplc4yi06AqUoqtD4Rea2j36ljPGffOt6RnZr46Mn%2BUZzkSscIq3igQ%2BZeSisvovgxHY8m0tLqgC%2BaiuCxUhh6kPThJtIlwkH210QuGPfyKg8V%2FWILZo4bYChr8US4YknCxTfX1%2Be5kOYRtdJ2vJE4n1jR8Qv5MzlrGj&X-Amz-Signature=b216e0c84dddfddd99cd4aa0c1a9442d036eda38de3f4ed83dc21468946ad572&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

