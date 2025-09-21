---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC5UAMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzmv99YQRukgJVJtJePeGARe1fv0v%2BpGZKt%2F0WoVKT6AiBrQAjZifJTDB1SgWVhl7LD3b2T2UwZ3hAt6BVE1u8uhCr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjmek9NcEb8CsWV19KtwD2cIptCmM1iATcu8Oq7A2VR7UKsiFADptrRnufCvkdMylbKLiLF4ChL0Hui8KgO77I%2BbuJX0edspxImbG6eL7HxsSF0dSaBe5xM3zn1KWTfZad%2Bv8bBXK6qIzNcCKowvDuFTrYTlzyZ5jVnPaaqB1bqX89NabvO37Ka%2F9KBhdUz2KRyyFitmPhT5kTf3ZJwpFXKlYo3K43yx4i%2B0uv52lAxMBQXivkM%2FetD3tE5vXvsUpsK8R5g4KYGPiFybvLGaWT0bQTtHP538KgED7BsuyMtByGyH8cB5tlIdsAd3HB9epnVA8nQ%2FtjbRQOhqfonJ%2BgVkFWMj2CRuSJ7W%2FsW3FIxVcwd7Zha7GsH8gwpxy7xj929Ge1MoLaX33Rejcehr3WXcRHYnEXFJnRALmDPnX3TvBkpterO%2Fu3BHvKs48mlRzf9LT%2BdItBas761L06BVq%2Bvbr7sqTjn4ZlfgbSNZpD4dub%2Blv8itVJnqH0ETG7yGC5FKus9DYykRYCt%2B0cyMdeTY7W2Vnr8vcXA%2BKKkxisQ2JYns%2BZ2iltbtfTUDdmJ2FwVnjMUg0wksQs1xCE286q2d7Fe6T%2F%2B9%2F8sVvJTJFe5ku2DRfq%2BJOZVEdpGqBIRzsAfjenxnqmuDBydAwnaS%2FxgY6pgGzcL8dCJ53NuDUT%2FAQTIBrBJpc%2BK992Bb2aecLzfLIRtHSq%2B9tWG1BxIFVuhvZF9fuCaVb8hXNAFfj8Ypsfs791z9VcyiDy2F2vHtuCvJYctH0sLGMzmdjDzHxAjXaAtf%2Fik%2F%2FNG97AczoYXniMhwm77fwvINVG1btYrkmk88c1eX8ZNynGJgjUDUnoZHK9teMQMh7AYzu%2BXBBByP40HZfqW6SiaHM&X-Amz-Signature=54c30635fcbeafe8818cc1a35786c9c43a9066475df1c37ef2821b062716f712&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

