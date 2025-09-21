---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663V4WOIOH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhgQAoiua6WGU3Y%2FN%2F1DMsB2O4fM9mfWzZ50ZZ29EQ%2BAIgNsogi0%2BfGjpWE1R1WAQzP3l%2F3xu7QyjjRRfnhzXwEBgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxqFbei8KYrNhG6YCrcA3FAGtNDQRWFUsGkBW3XSTz5zKSwhZ14mghMKhCcwcLfx9YOBzf49poHIqwtDXvZqVCKWR%2Bkkia5IZBUIH%2FjghR7onJi%2BSmqVU7zrow9f257R3Mj5TyTwyms2wQlovu3iO3HNUN49LluYSVim8AIr1QQov68maxWcEAA6%2FD5Z0UBeLHYwra17%2FcAbjEx02lCF1qbVUl2co2qvqIJkcJPJZsk0Ki5x7DyUpUrU7NqpvHNfTkpFkeVB8seTn6Ds88TukW4pZLM0y%2BF0ZPfMDma%2BvUiI4WN4bkhqtYiteKOSGcnEhyMexRWZwHmC26JeL8Ohol4fQrOLWHrMWGL6XB03O2aBHAFUeeW0N9C6EnFi8WuYs83FdIe517XHVBpuxU1LnVCdIlbMcJI%2F%2BDCKKptOqnv34yoo4dv3CCz0bPaVvLSfYZn01By2ldE1PdnE2djim2DzIGKrmrnd3zD5KjP2wKp15Ip6gqaoCR4PpYRuJmnJeLVdOkXKvWYr70jTLaneYGYhRFl9vrJnUpNfivYpKm42ZsneLCrePT%2F8Q%2B3OBWjepoEziKs6qSbOe6I%2BP0FnP5%2BXsC3X4%2B9yadRUIfJOOYaG5bUCKK793TDR7lf1u6osTpSrUYLLGScEMmqMJSLvcYGOqUB3a0hvWwx4N5helFzXZNsMoQ8zBZnc71B7zjsNUTEJI%2FJhP40DxqTls5OYx45I94laDIAi5XIZvBGh6XH7IkYUraqyV497JuXplQ6r%2BcCr%2FVeiB%2FU4PauXQzowRtaebgTHAaLYQyrj6vK%2BLuXPf6D5LV5ZKZ8nuA3kxbCoImRsaBlVG5M8zOgvvxsnR%2FK%2FM8htuCQ%2FZ2L7hQ5N%2FgmOrNfHJaSDMiA&X-Amz-Signature=78c1bf65764e64818b1bd50e3bce9cedd934ba149a29fdda1b3159bfed4b6606&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

