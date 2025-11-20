---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNQB3OHD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJIMEYCIQCBvwA82F90mxb7%2FyJc9xMPqfARID44gKaTVU3NXglnjgIhAKAgQcocSp7qzS9EA1hyuKcMTKgldeBAvidanmXARSTtKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwyuy73zWn9uampfncq3AM4MT940c%2FsmOz9mnh%2FSI2%2FtFAKXrEGZmrcXPlr5l6ThOQ9Q3gviF02UzWmBKnZQbwfTDOMo5ukHRej9K9hUbLtXqmYCoNozgXZPPzQQ8mOeLrGuZ95zUMHm%2F3MOCN9toKQkxIpozKpAH9ZUGB5gprE1eFxL1mZuQhCdk%2BXSCD%2FsSMQZItSM6LDsEXLLdGL%2BMaadorkwtVZbwQLOjIe7lXG0TAOm7EkboE6xQSI%2BQ3T2MRys0FD1qBrvo79oGgcHyqh6Aa%2B3XBCGNuXSBisW%2BeyInGUu9kKAYAopBXKI5TWMlZsgUOKH3UoPURIVDmpR53KTlFT1oE7M1iDuu%2FsWFFYOn45Hir3vCHgAqqUblJEe%2BHWLt%2BYeK81VKj4xpx9sEzkuhIk%2FKd8Z8GAsfPacu5fKj%2BJePCmFTU%2FoH8YDmzIwjJAC36e8r3DttY0xS6yvi2tj8J4blyQLVC%2Bt1pckz9jiz0GaRauQRuwrCTon7poHwXu4X9uyot6Ei1hcEbiYCGjS6kRvALv4dtqz97a5v5n7hZcOQU6I3aLyvqoUmyu6w361HoXdbP50hqEke0ZsAbtUweanwXb3qTrxBV0xkbpUi0sLWNuVaINBAU%2BF2i2PEsQivzdjpn2YduSOTCy%2F%2F3IBjqkAQo4fXuVnYfOHl1Pov7ZFQSAeXrDsz5Zy%2FklW45oF21NuSGSVUgpatR8Yh6Qt92XwylqfB5msTP3v5CZkDDfLXcDeoFl77eT4Y6JMwhew6HXtx39rW1vvGqnTDp8TN%2Fh5dmQ7IXt5%2FvYgo%2Fi%2BhIGpjaA%2BF3t3Bf%2B5lKQgNkGJc4HeEmkfdZxgQI17lo%2FAhnueH1b0CTqEtF90y%2F9US%2FcCvIBSRl5&X-Amz-Signature=125e4e5e86e2162d32b673bb67317bfec9d2c3c0b6e147e9b19171f94cade206&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

