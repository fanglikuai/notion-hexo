---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNQB3OHD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJIMEYCIQCBvwA82F90mxb7%2FyJc9xMPqfARID44gKaTVU3NXglnjgIhAKAgQcocSp7qzS9EA1hyuKcMTKgldeBAvidanmXARSTtKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwyuy73zWn9uampfncq3AM4MT940c%2FsmOz9mnh%2FSI2%2FtFAKXrEGZmrcXPlr5l6ThOQ9Q3gviF02UzWmBKnZQbwfTDOMo5ukHRej9K9hUbLtXqmYCoNozgXZPPzQQ8mOeLrGuZ95zUMHm%2F3MOCN9toKQkxIpozKpAH9ZUGB5gprE1eFxL1mZuQhCdk%2BXSCD%2FsSMQZItSM6LDsEXLLdGL%2BMaadorkwtVZbwQLOjIe7lXG0TAOm7EkboE6xQSI%2BQ3T2MRys0FD1qBrvo79oGgcHyqh6Aa%2B3XBCGNuXSBisW%2BeyInGUu9kKAYAopBXKI5TWMlZsgUOKH3UoPURIVDmpR53KTlFT1oE7M1iDuu%2FsWFFYOn45Hir3vCHgAqqUblJEe%2BHWLt%2BYeK81VKj4xpx9sEzkuhIk%2FKd8Z8GAsfPacu5fKj%2BJePCmFTU%2FoH8YDmzIwjJAC36e8r3DttY0xS6yvi2tj8J4blyQLVC%2Bt1pckz9jiz0GaRauQRuwrCTon7poHwXu4X9uyot6Ei1hcEbiYCGjS6kRvALv4dtqz97a5v5n7hZcOQU6I3aLyvqoUmyu6w361HoXdbP50hqEke0ZsAbtUweanwXb3qTrxBV0xkbpUi0sLWNuVaINBAU%2BF2i2PEsQivzdjpn2YduSOTCy%2F%2F3IBjqkAQo4fXuVnYfOHl1Pov7ZFQSAeXrDsz5Zy%2FklW45oF21NuSGSVUgpatR8Yh6Qt92XwylqfB5msTP3v5CZkDDfLXcDeoFl77eT4Y6JMwhew6HXtx39rW1vvGqnTDp8TN%2Fh5dmQ7IXt5%2FvYgo%2Fi%2BhIGpjaA%2BF3t3Bf%2B5lKQgNkGJc4HeEmkfdZxgQI17lo%2FAhnueH1b0CTqEtF90y%2F9US%2FcCvIBSRl5&X-Amz-Signature=be5a152ab673dd96ae6100df6ac7225218fdddf9f315c0547661b77a8c1ff761&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

