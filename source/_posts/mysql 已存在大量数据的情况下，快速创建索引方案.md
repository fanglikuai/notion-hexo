---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665M4D7AMF%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T060052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIE8GAcJPKlws0QS5GAmEJIDEhYAK0JUzwMAeh6ZTSnigAiEAkC6B9xhuPRLT9BEsLXmEuJit8e4aXhzM0CxEvvbmnr8qiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMurD4fXq4bqCO21iCrcAyT9i7BePllSorrBMc3F79dA9Ezdi9OdCnfSzzvckjcuRr9mLOAxXPcEe0rmLkuec3%2BTuyKNANlfhRDf7vFos2awHvzu5WGwzyjEroVpvxX2m3pMnC5eljiNNBjqFoumwiZjk1HForMoXAUJRnaW7LZejMADcvyOKJ6w3MPNDYanyUW%2BQ9Hz3ld10%2BA3hCqyZnChR0ThP522fDzpOrxrQ%2BIApFqfS8FPL41EhPGvmiOEWJhbigadFpyQ8ebHxZzYAc%2B7xbXdCb683SRnH6k69qGnPicD3EmA48vOqs1D%2BRpL2DYoru7%2FXGT8EzVsLhwpWdkmxSpB%2BaeHzUY2c77MYzohhhEU9lQ7C%2FbZqDU8h1DXTi01PWO2FRA0X%2F%2B7NCePQJwPNWjB3dIU1nVkRZyC1TVrkkd485af5yCW1Yr5STBN4X6c%2B2CFtg5yvFVPDZr389ZNb6Pag27LEehjxLACcbUBNzRyAadFjo7AU3mvCbYSOLsY8SkqWsjp%2BBYodcJy9GktcejZYRudK9fwOGSSFs3ahF8XTUwMz%2FcgbXl3r4ot5oSv%2ByUXad12diGUOiv2B3p3QmVtMPEXzRQp8UQKXyjQIwqGrouxaPY2xYuYN4x8YZaEL1WlDNb63a8cMKuvu8gGOqUBhLVEPQRld3SIBNwXItgvnzEgayxm1yMpbJ8nfyxYPW4%2FN78R%2Br4q%2B6jGFgvLKhjXMkCUeDU8%2FABWdSz%2BSdBkYaqMePhIysX%2F0gL8f4r9I8t%2BaSCPTC5Gl%2BRHNre0JBvixZFH8vWi0Hi1R7QIS0Fs0ch8SdP%2F1m0OOCotEOSt9%2F8%2F2MMrxq8MLaE1FGD%2BGG%2FY5ugfBuNx%2BsV1huk8jujzQ2cwd7nl&X-Amz-Signature=09d2dfd455064585578225e7f1bd4b05538d2369fc3386a86991749e1f2985c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

