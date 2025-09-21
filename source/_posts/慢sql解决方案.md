---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC5UAMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzmv99YQRukgJVJtJePeGARe1fv0v%2BpGZKt%2F0WoVKT6AiBrQAjZifJTDB1SgWVhl7LD3b2T2UwZ3hAt6BVE1u8uhCr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjmek9NcEb8CsWV19KtwD2cIptCmM1iATcu8Oq7A2VR7UKsiFADptrRnufCvkdMylbKLiLF4ChL0Hui8KgO77I%2BbuJX0edspxImbG6eL7HxsSF0dSaBe5xM3zn1KWTfZad%2Bv8bBXK6qIzNcCKowvDuFTrYTlzyZ5jVnPaaqB1bqX89NabvO37Ka%2F9KBhdUz2KRyyFitmPhT5kTf3ZJwpFXKlYo3K43yx4i%2B0uv52lAxMBQXivkM%2FetD3tE5vXvsUpsK8R5g4KYGPiFybvLGaWT0bQTtHP538KgED7BsuyMtByGyH8cB5tlIdsAd3HB9epnVA8nQ%2FtjbRQOhqfonJ%2BgVkFWMj2CRuSJ7W%2FsW3FIxVcwd7Zha7GsH8gwpxy7xj929Ge1MoLaX33Rejcehr3WXcRHYnEXFJnRALmDPnX3TvBkpterO%2Fu3BHvKs48mlRzf9LT%2BdItBas761L06BVq%2Bvbr7sqTjn4ZlfgbSNZpD4dub%2Blv8itVJnqH0ETG7yGC5FKus9DYykRYCt%2B0cyMdeTY7W2Vnr8vcXA%2BKKkxisQ2JYns%2BZ2iltbtfTUDdmJ2FwVnjMUg0wksQs1xCE286q2d7Fe6T%2F%2B9%2F8sVvJTJFe5ku2DRfq%2BJOZVEdpGqBIRzsAfjenxnqmuDBydAwnaS%2FxgY6pgGzcL8dCJ53NuDUT%2FAQTIBrBJpc%2BK992Bb2aecLzfLIRtHSq%2B9tWG1BxIFVuhvZF9fuCaVb8hXNAFfj8Ypsfs791z9VcyiDy2F2vHtuCvJYctH0sLGMzmdjDzHxAjXaAtf%2Fik%2F%2FNG97AczoYXniMhwm77fwvINVG1btYrkmk88c1eX8ZNynGJgjUDUnoZHK9teMQMh7AYzu%2BXBBByP40HZfqW6SiaHM&X-Amz-Signature=80355cb944bc832a4e705d27bc1777c64afca305c532b80e8b327ca450ad3e36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

