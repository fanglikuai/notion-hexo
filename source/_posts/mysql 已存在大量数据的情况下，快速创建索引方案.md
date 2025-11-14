---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S42TD4FE%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB1AaoY8Wi6Xr5naSAWXVNHtbw0LPwUUOtB%2FcclkkEbtAiBfdfcsv%2FlHAMnIUc%2F%2FmZ9nJ9CRcdtKlOsE0aK3ygWfHCr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMCgzO3dlNMOF68SMyKtwDKtzHn80G1hHieOgh7JJlaNLMEUV%2FK2v3DSZS%2BC0vt%2F0aLCf2GG0UzGrXfeYKk7dNRzIFebBBYFtEeDALzeyhuN6RmcZaOxfQFd1BYlFHSYi57qrDKwH5yjuBsZVwIBWl2FJ6ipx3eflDgEyF46wBSYeWSBcGK1lG1%2F%2BoA6IWkLC8%2B0jdIif%2Br2svAbCYzIkvR3ED%2FoFNpfKFp9ta1mpnHUcWGIvjaGjvNj0bRk59A84%2BprkqLobgA1nBmlfFAgtmTVPpb1nfNQRf0ETbkVTuiB5WriuTGf3UcOep2Q5TCHJfERff%2FtA3rbhuRDaa9r%2BpTrzsjYXZjr%2FXcDw4b2I1zyKxZ5xKRGD4oQR8S3Zn2WFh7tFMec8KMBHa10Z5uvQkWh1XMikLIk2DSq8JExfCE7cRj7wLVE6wBAFE9a2UoY4nzDGJaIVpb76sqjwmTiUJi%2Bs5HlcxYWpWFx08X33HCV%2B3nLX%2BpPHh0b8itBhUlo%2BC16mFOrCW0AQrhyr1nVyUC796auyotQTS9UGDqHxdPMShoXq7lHwVJ9AGxronHbeGpUA1bHdxBv4z4%2F7CoQJi9E8lG%2Feh7%2FUZmrsrxe0z1ygaOLAGjYZ%2Bv4llM7vc6E2pZP5%2BR%2BhA2XxLIGQwlp7cyAY6pgGJBrHTaVH0RQFJTLRW3cVbUlY46g42ICXbjsSUE4uhI7iU9yuZ5kEie1spg3uXXHQoIvPp0F5g5%2FPrpKADeua082mWCCoNONKYQTmjNyexJ5TkOLe%2B4S1Qx8aAj3P22wXPrUv7539BXYKyBF1pRn4%2Fs%2Bnbgqrw%2BAg6iNe9llvtElF3DvNXCCcMO1nr1rYdOaoKhOrauuXsot7%2BhA8grsU%2FXwyVXbNX&X-Amz-Signature=68910fa369439b17f40f09bcb374ed5023862b62e4a4759d93becbf26ff8307e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

