---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632V5CTJC%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDc63EWCU005y9Dtw6iQNozxUc6B5pi0wCIDRoJYdoHwQIhANP%2F%2BuiSH2OA9f%2BvujEvP5dp%2FbcAgRIiZVnKN%2Fh4qQpYKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwByJGWdc8Kk8be7Ucq3AO3GIR2qWX3BXHRaeAo17Xrp2UPY9riN%2FC4y7g3TTANOKjgRIIjyIHLziPNFd%2FuGxSj2eH0IJglMTiIhsMr7%2FmUbL5OqHBiZ4CO8i2OYPDU0p2HYMy7%2FIWTPeEONREenChZ4lj8e3VFOY3MnmB8XyEGk%2Bdc8bZoG7D9DVtEVr0nrFpEyT3J0FbvpcA9EtFFbq7woeH8wY1ERUB%2BZvJzIJ%2BOqDBf%2FjiWettw5dO53B3XFgCBBbX92BmJntVDOXPD53zEROelVzIeIZ7ygN3m7R%2FcjrsnYxkpsMOT0KlEKNpQ2kEToewY%2BZmwcgDq9MVJgus0UpCBv9z2TxgLU5pWElllJihOmQ%2Bv5BFsR937cV0wnnHUG2VPmW9ElfimiW1EzEhM9u0zA5uJvrUvmMWn4RG3tur%2F6jjOMom59fDo%2FldaOLY3TzNnuLlfgj0%2FBZg%2F1ky8h6xfJ1DRzCYHlSuU6TC27UNG3sHlTsaDAt113Zy%2FHRRLeRpBmyZeJXE%2FeZJD%2BGlVAsfhwGJ7fYem%2BE9%2Fvx1rQrNYrx2XRNVTMg2zEZwFae75j22%2FTQq0Pp0OTLYjDNpm2oikGmbX72bMTASsa1hljMkebJoevqSo6uTFUJowYDJfV5N4rEXET3sisjDOzf7HBjqkAZXhYQ33H5%2BHKmrl%2Filnmh4O%2BFK7t1GXxODNs3QVJWaKfOTK6hZDrbw9PMDIFIjOPnSaUSGRq1b81SBwQL34qGfXReSC3CueDaBxJVR0bhZa54qlSrTULC9bI%2FUs7bvg8roMeQGP7V6zfXdfp19KqjOb4Wz%2BWK9XXruCsfwOF99f0la3yjHbm8DRqRKW8%2Fqw8IJY%2BmvBDL4%2F2H%2FWG0jgFQRzuARW&X-Amz-Signature=c69bc2615aefc028fd53b13cb2b622557898226179e7e6194efe674ccb2ab3d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

