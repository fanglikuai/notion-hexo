---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665X5ZEFOW%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCIH1UeyrFZmrMBHpOn3lH%2Fciu%2FDLRR%2BdbbfTDXsvlu2zpAiA%2F1ESldAtDnXl1EE9bvJ3Ib89pJSM0rU%2BWKHiB1xslsSr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMusTc1aw%2BI4ld7HrbKtwDuSqo2ijk5NVRa3J0s6COYL0CuUlYH63q%2BCGlTsWKnoyRqFfMF1Eg5cr9yXTieGcVk5CzktG3tHD3X6%2FKZV1E4Ed2mC%2F9nlEyXvrL4QcFBagQzhmIiw1KUbYgV%2F%2Bp7HQAuzck2oazg1tuYAiqXI59GDK3zL1iHIS1vNioio2V%2FDRvJIXRyYGDKeDHjsMJwUzjtEX0ktMsgixbZZ5Slurop9vdosuoGx7mngzHT30ASzkUfERonJspEkQuj6ZScEcGVCCsnxArQk2UUcD%2F6vBCJnLfwx37Ca36pcVBZuxksC4%2Ba%2FFJSJ4NWqo%2ByWaffml7ZfdHfAPnIF1x1AfBwcZAA2neKMfjuMU10TQQC4WoZ11gTsfd3NXT6I5xstSKEUxuhM8G23NQW4Ot1tgBpY9xtAUv1A5EBnq8gvTzhAuJ32KCiS3eW%2FVbrMGmEChAte7wmRYFP3cgQagjrbXTtRTeU71gAbCml4RfITLtZGmzXpsg9meADsvDgUbu2JMmjnMVu23kpR9TQ0Kg0X3nXAud9hMhKxcCyewHKw9ZV1ttOLxZxD44%2Fh63NCLTQpewCfo%2FHBKNYLdMrAR4L3z80sNoY5VgIWmfi5gfazWxH5n5%2FOohJiuMOQPk%2F5WfhhkwuIiDyQY6pgHY%2B6RITdnKl%2FSqZTxatNCXHcgOl0qIuKZqdxRarO0Uo9NQs7uqeMmxmEM3M2JEC%2FqMLNkvQgYLSCMRYS0MPhGHv82Sk6Gw2rkVV7%2Fep%2B%2BRePlokra0Tfi0shJY84jdVINrqM%2BnCte1t6v8h%2FxMKWDNi%2FyBjfvV9RCB8zb1LCwUjVxDRaislDoPKIizdSUq%2BWR%2BJRdQc11N65LEUymXtVOkI%2BgCxxJn&X-Amz-Signature=3aafd32cc6eb2eff0679f3c324b3d01f73ce48d0fdf41f6d6e2d15c9fc9dd85b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

