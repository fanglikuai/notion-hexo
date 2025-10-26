---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GTQFXXE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz7RkcmxI1zbhFiIKuvmCiBBPFLm%2Fd%2Bv0HPVMx3iZw1QIhAOq6xwo7GtHp7VyQVzr4vNbv2G33VXQ761wcsuVsWDVhKogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxq3O1MYdwYOgjHMYQq3AOvBk2ghuSuoiaDT1CqKHsKPTaWUwvZrqeJ4BJECFeOw3crouBSIGfkMIs3a7v8vzdvsW6GO6CumUbJWzLO3NCDKYY5T2BxmsjsvlMECNfnOqNJl4v%2BDFeHF7uV8NPEXuFfSm1Q5gsiMUOadIlS8eE4T7wrtDVedIc%2F0fXDsowXMDsi2UM0DrWCPPI9M9wP8Htl22VBnJQMuyAVjMhY7WKdKEzhF0pOUizXivu5tC%2BgESdVtHuQ1EFuU7qUuJgVjtaswwL2tHFQtpmjdfIwrCTcsiyi%2FNFu8iRuShRgSUwabj9dw5sNfYyi3ezmVChAoqWLNoS8tsE4G9eU3VS1sRaGZf5wpLS8o7PDsdkphrmXSoNj8WJOBn9qKnFzYEkmDlzSRjPCUAapAAjk28BDBWijq1zGb06Ib6O35XFV2o70x2VH7lMKmegN3N7yKOyyXptvKSNc%2FlFkaVG%2B72mFm5TIabP4HrvfeW81PPlITELHpqmD1LNJ3J3iZg2YQhomB1RT7Xnfga594d1DlLWLnQxb3kHvv0WLUPRVxsf%2FUb4xgBmaQjupAg6rI95u%2FLKg9Iln1X6dD1pjNOizSisB7IySrfeWSYy2sb%2FtfxhdYr7uUQgociUcnigoA7%2BcJDCxkfrHBjqkAb69GFkDdCj0thxvyaABol8LPO5SwO4s3TD%2B3IxcNduj7mSPsazKLUUCEEoBB7YTovkjg%2FR9AJDF12T%2Bp1B64rdm4aCmLpFkF%2FZYO6SSyXWiKHYnsMZHBK4Y0OCRr%2F15L5bQJ9gLFeKMl4rckdwKkEGC7jpmNbWij5e5jioKUtrBTtLmkypc%2FrLc8AhwpZk8wbaqDQQkiOYfy3gQ5jCBxHUssans&X-Amz-Signature=60c391220bf5e96830c298304eebade24e922ae1eae86644d97a4497fb4782b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

