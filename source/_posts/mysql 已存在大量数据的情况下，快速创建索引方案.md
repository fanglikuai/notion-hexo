---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPXY2UUL%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCIHcLPlzo37sDsIXsgnZMqhEoAE8K7oiWx%2FO8goAqdoQIgB4BHA8HtcXgzGpayexBtlbYeljjpBkCpQZJiyDYuT2EqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO2vfVj6XA4QFOcOYyrcAwubzM68A8UiK%2Ba%2BcGnyq20kXhfAED%2Fd9CvmW%2FudSyrqukLZpjoY57yejFNyw0dN1MxmcPCcQFeGGYxvoUrX%2F6hfMkt6aEN%2Bkuol3cdIBUeVVrHdRtZcgTlulH94WoTGEpvEV6fiz97cquuEGoMrA4JvHOZ468yRWlD0h9E5XpevSpYsmwg5nW6zZsptO%2BfZWc%2FIWbx5qyUsF3bhmKm1X6K%2F5aQFD7T2f7RDM77rPaEx2Msw6lBLEDKECTp%2BNkxRs5f7K0qIob42S%2BWVC%2FBMfAs8tnZtnhsp7OaO0NRXP589zwyXnQcI5%2B9N4LqVAjCJiFexXjAbtcHCksKsBLs027XST3k94NlkIPnvVCNvTDMHIBBo3vcEQLznDtHpHA3R69fVDKcha8e1FXLlUA7cbSTSNp6ar5rhfQ34KRRpaqL4DvQN230J5%2BJJT6hr2m66qqP%2FoQFiL6iPO3%2F35JCH7H1Rn0ahn3sx5zNmZk%2FZpBy37mey3EZuVQgB65LxIik4EBvqXeWOT83d7jgnVtfwyU5rw7loDXDEpfnyI%2BSG8hTVEwAF2wVAJ%2BIsNg2JvHkTEHKVIdn7SXsc4rI244DfFChvhWTSwGmyPVOvXrrxTgve9JGy%2BFSbr66UmgoMMMDBxscGOqUBkROLlDZH2mjdLQ62NimNC1w%2FHAFnUCtTKezpULT8sNXvdFT72tPvPgfG5WCCrVh1XFtSnO%2F050%2BX1DtvkePHLIdQwe6L15qUpIXVs%2BoPv8u2Xredck%2Fa9XIIuIEEmOs7lLY5CdSJQNZZUAr6Q6wD6Zhhqa8nuUFjmdVKM05Ehh3uAO11waPSgcsi9woH5m7MhkAehljIwWTpN5pvLa0DrpwivPVR&X-Amz-Signature=7cd2300418847704fd54b2da28d31ab4a997cea7017e9bb86e431bd5f0518175&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

