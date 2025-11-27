---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTQ2EGIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGYyHSA3jZczdrRxbVSrfPf3OyoNny683fhgHiyhhnhVAiEA6tcdg10WOCUbPafWHtgeB6IYSD2haVDyp406vlQVHQ4qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOt1LMSfAZih9DO4GSrcA2liLvrwPVt4G7IstTcnytH8238vwC1cyl8cmhmoX8Uusu%2F%2FDWVvbg9TCoIXZ%2Fdo5wu0vbzguIuhGq4JrxHG05fFcRKW9GrGCcUWsnPkM8EFsX4wBxDe2%2FrykMsq4NJHOQ7fxCTLitm%2FgsdWC21fefyZ4AlT9i2F8MpvPOalx4BVrgQ4E1Vqiq8B7a11VXVf1j3Bd8vKQ8jV0VtXl3aUy%2FYDXIILMfnEer%2FXzncDcCPzYVmsbNNQiyha7GSho6yMRn6rsLWsE07FwWo51Rqy%2B3lcDBLJPbGIzPSblqsTmdLeicTRJJ3wOdGoFtUkXwyOGGj8XdY%2F0Fgb14Xhbmw1KDujBqfml1NRpUih1ayDP9eOCcSIuz%2BhobL67VYNzlbwoDQgvUayKBJ9KXMgH%2Bria0owvGy5VIiRRz5xI5MLIMz0maQ%2BDpbDD2EuHJju0BTrwJiAYo5zh49eJCsNFmWXioFigARfPwBliH6YiW01Cu8StIR6rL5j2kVFczAnWutOXBrcDPoRIe9CxY4iCcre%2BcWBLMqrbSj8%2BNhAFphEyvIaX1qzrNoIk%2Boql7Vx0LBjhg7JlTnDY3CF4bDiA7sdgV8HUzgXPaWWd7qxuum27h4hyhSXjlE8Rd9XWIH9MKO4nskGOqUB88isa8XrHbvmlj1hmROapACwvT7d787Yz8pyp49IW0HZhagzjz2gxGiJwU%2F8q4U0oDnpL1AhQEBq%2BH1ftJQH9lWJjhVkUNEgc9dbtYcZYRYNrQC7TZo21FNfSkO1LqO6Nhvqa0W2cdfAcaHQiG44Uzm15VwkAr0xMVGp0LKesAC6hJHzIb6iHZUeBbGyptuqjEIOMAK2ptmMzLNT5iub0Kr8wL89&X-Amz-Signature=4873ea7c3657f9a2caaa81cc744778e2e24add332e0f1ea27103f341df6344ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

