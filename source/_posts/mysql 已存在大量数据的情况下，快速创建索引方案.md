---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3ZKL7US%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICu9yVcnD7kQMwAWrXoZDPd6aFZykNzjCyy4MtvEv70PAiA%2B7PulOPM2tSj7sEG7blkpMS5EIoRSEE42FaSKVSzsDyr%2FAwhTEAAaDDYzNzQyMzE4MzgwNSIMD0xwF%2BxxW1b2veqAKtwDu6T8DcmArpVhrIbHkHPR%2BzxG0Jg%2FM8em%2BcnMsZLmTk8M6WUDduGvEkFmg1yBe6azmwzI1GSPMpZXskii5USUL2B2SdRwFiNjrQ9PqyYup5OFgBpJScXqU4Wp2xRcVk8vVOvsaP5g2eW2MN3vmTwtnq%2B%2FhdISOK3GJTfTwjkRQzHpLempy9zXa34jxgYlcSARCdgl4sWdlJjRi7jl9ua2BnBk41nZKyXyk4KOKMmZrMB0CSahY6DUgijkEoBL1Hm9fYkiLktY5Bp2LcyrcWqTlMpyBQW5hzaqFcdu5JpJ4qmhUK8VzSb8l5jNZPyQzKHQlPqYA4vJK40mS1iAEeX8Cc1XO9Rtw%2BwkSNuf1aS7DvtuUrh8NilECgOn1CO68gz4dVUhcfxfDNONVLHBG9ZUXKQTV27zLqos2SZL%2FQvnZrjgfQu9%2FYeXJ5H5nkSuuwZv%2F7n8Egd9x1ArBNkjnNV1u3%2F5rTmb260z427TOwjWYvx%2F%2BW%2BL65rzhMdlVd5W4482ttzpIdqDNBgitQolQhzbpT8RZdA2LXzVQs7GDsmTzclcPnq7xE%2F%2BtQDgPYs%2BQwsBU5YWMOT9T9a9FU09cldtBfq0EFCc4P%2BtxcCamWTsYNScWfjs9DKGC0iIa1Ewm962xwY6pgEGJGAJf5U4dxGaaducOJICH1gNsuU6tM%2BE61B542q5Yl8ocw8%2FTQjw3D5QX73fBguFPZfkjY4DJGM2HMyGQrZ8%2B%2BNi3iafYT6fo6hHntAifjEE8AXvpoJ7dyhNd9FyeYzbj%2FBj3MPFXYhHHvCxTYDIYiBIAbD0mT%2ByEs9R1qxS5Jg5CQ5EFJ7wbrAdeoPqXybO3vKWhwIdTOyNG51%2FepkJrYRztPwQ&X-Amz-Signature=d9a1a417bd5ae94c3d857b5e6cf43c645ecbbf726d74c32fa6a7626e1162adbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

