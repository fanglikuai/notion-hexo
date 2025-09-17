---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNCZADRK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIGHApIZqHYhQRUd5VnaVBtY%2F98Hcc3MS4bAPPgg9f%2Be6AiAo%2FyhkTtKGbiyqwsyqUbSDq%2FUxIz4LhQTdsgyg9T5gJCqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXGJ5xFPBiv6Jegr2KtwD%2F6JSuX1jtKbvFyj79F0lYcOy9kx5XdSm3%2FzeUm1QxXNFwFMQHLCargYcSYkkFNSYcE7s2NKOa2dTQrk9i8GbK12ceIkS8R5UE8tkH3Aivq%2FXAiicCA2UUG127qKPbKgpKiIG%2BgAK92hhSjG9eapOf41rPmOYwMSgoQMy0C90gMiLjARFccgY28YXk%2BV4PEyUnuFzht03Zs%2B1J1oJBmwjmRxtyA3Rp4xttGGNEyFXayOP8B2ptDfPpZnmtsgoFwqQ1ajaP7VKO46PBa46qtifM%2Bi16ymiVIWioGFKU3ShMpGN2nda5UG%2BmjmjKPmAzxHHoAQ3SCzTYhTkJQyNhNxOSh7WU2IC2aWgIVNZNMGnC3oXJxeFAJG1Dc3G3rmR5J0%2BpduClGcKjtvssKxmnYXxUCsiQYoSPH2L0dHutyvQ7eqKrXfwgKQMxCNUG%2BmBijEkTo52FwxsjFHi5irVmQbgm%2BJAQKb4S%2BUFZGOlwVi8hDfEE37ong5cL1XxcDqzsx%2Bunw6esA3CQBzqWoA9tM9UajAALTIw%2FsFkkSb4X2fCW%2FVTSNfNQvSctgT5ZKDPxLxVvFZuRzEvb2Tk0xhf7VVW5RkvzcFMLWEBgZBEi16E%2BdxDFih4G%2FYcWzJBO04wnqyrxgY6pgHl5h19K829lH4NmrqmuPE40HumPteLdlJJQP69UOlBWoDh63pYsK8o%2BagyUAm2Ri7YJu1m1GlUcOnCXU2X%2B0yOjs7RHMyhSv0EPeZzFsNWBRFzu33eA2JJjP9p%2B4JE3zT0XBGL9uE%2FOYMYHd5957EbdHQ2Y8mb3ICxXTFB5ocksPr8DvAQMw5ACSL99NCbcCdEp6ISOI%2FvDsfKJSHLprQ9bwDC5OJx&X-Amz-Signature=3972aa8f416640e5cdad75ddae3ff37d5d4c5f3ab3b757342df019acbb6ce30c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

