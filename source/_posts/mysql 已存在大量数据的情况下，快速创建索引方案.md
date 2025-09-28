---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIMESF4R%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCpSzF8NUsX3ZfEtZI57I1afX1MxAQY4Fqa16UIpzTZlwIgeRmTg%2B2RLrREcgGmexLGeOKMijiALX6U7%2BHjjqTHqJcqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGDUPyxs6fiXNt4AByrcAyimqL7MbTVmFbLA9DKf%2B%2FOAvhUTSG%2FBmPwUnNOx%2BERN1qrM7XDSUvFHciBaI5gDpENz4gzZAxukGHdrDDQxhH%2Bwg18KawGM8z5Dj%2BoGMxrRNsg5KxQsdeNpJWEyNov5RECVbR8ISmVXfh%2Fte60nQIXgCtajWm2ibuTX6Un8UbiGWorPLxnB1BFAGsFkraqvkgoyUa7NkPvvyvMbZAhTvQ8MwWixn37%2F7X4et46Uesu0SGDabf98HOCo8fqVIhCkQkADu%2FHcCH3muQC6fcKms4V8%2B03DaackUVY04nZYYc2wFyDR2%2BE89dRrpEB3ETPvXkvLp3HTQAxuFr6UpTo4wZkVKBN5X1S8485q%2Bh77nSPWo8AQefOzplpiPBTEZMgUSOXdijAhHbSbJUcge4%2BkkvtrTfPoYZVYlXKATMIVgiwbq0QdK9QdYlNS2rszQf9LQlW4z0YZ%2Bm5Pr9JwKNsxwx%2FBMi1aJ%2Bbt56QdMrrBV2VAwCYxeaEnK2heT9ctznp6MhHogTwrYvuwpKh2djjoMzIOSMp8NwydFq%2FoXu1ccLg7Adgg1YnYegPxfANQ%2BeZBCV58G%2Bjl3TXZ2cgyjfP1CmLU10JKaCLBLI%2Byatkxs30RxDpR8H1DGmiKA76XMI%2Ba4sYGOqUBcp5rilZCI1%2FfqJRFX1hcBLBFGfKCpKLY3ZNtiy2roU94T3iUhjDhCZCWXQr9crdmdPdnDy1t1Hmw97zXISVJfj8RopPq6Rqt2sFRSl26FtIsxgioF3t9BJyF7F5cvIiQ73a5gIAmiobOJkbgXiSOgbVlN4OsWf2FtDtsnwY2wsHhqmtfxTh9X4bGMEDUeMapbCr2XpZ8NKS8sl3iu850PJ3HJSog&X-Amz-Signature=f252c1b24b312ac2c476bba575e2c768d0bfa95a3945d8eccc2d4e5304078dba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

