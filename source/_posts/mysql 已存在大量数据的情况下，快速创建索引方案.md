---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YV7SBHP7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICmujF%2BGHHm3lOP5SW%2F2wp3Jf9L0F5c80rfAoyX%2BTd%2FuAiEA3rlTBW2U2y5Hm6D3lHEGpADi30iOXp%2BoMB%2B5MuCy5rMqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIydr%2BQtcqfF8LiOJCrcA7WEgkadO8sOI%2FC2OjTqo%2F2p%2BcQeKtgv2z2sNWqew0S1QD80cb2w2Qk0OCLw0w2wQpMZjwta2Ev%2Fhakj9pqOeuhMb%2FkXMBHrJuVMCndXaEaFnrwaouGd3r10YQIrhjIpKRKhdS948tg6i0xqtqM7dLYGYQXPn5obguGcjp053fP%2FEE%2FKrjS5qDwDCV6m3Rn8%2FNm8eU5D92sNHM2r1jAU85xGBUZcx%2B5tJfSaYk0jaSkHP%2Fhfg3elV%2FQV2rWrARw3PH1sJzNIViSjFcgYmQfWm1liRCI9LZ%2B%2BDnSi2GK3zRpwnvYPyk3cMkmLd5eqBT4WnHRzazhYV9rUuwgFzzsSpwUOjssZFlHlQ91zUi%2BOGHeQHwotOg5VOpCewXcMBdUpHI3aMpF%2FspGc76HGDJ7AXjRmQkQXCyNSKukf4d8r7yrJQEDutRTroJPo%2F10QW%2BjdlNgcRlFtRBr7o7JzSFqop0x029W0w1Q21aNu1ql%2FhT2QQqSVZulv872iAAW6yg4p6v%2BSAMnKnjwQoaPedUH4nM2nhLJ4qEtekFlhiWiBY1ainEhy9Vb4OMXDi4LmSH1CBiLVRE1EeErtgHm6UuTkzAdEspgrhJWASBCPVGz2XFECd%2BPj4i2Z1Ra2LiJ%2BMIiekMcGOqUBZbCoqvbSga8YuHO%2B9VhSVGwA%2BC0WZyiZwwnq3LPq%2Fh9DSNvNb4kp6JFVupCp1Pu40cxJPzJzTLqwPb2bfdshJVKkzobFZquh0cwCZ3AQ%2BJW1azucH44i2GH2afBkiTMZGSJC2W9PGooWNok1e0YlHROaSMphE%2BF6T0pdtSijjnk4fircsySvRnUXaJ4d5W3Z6iT%2B7E5Yp6Jebz0gOtOfLty%2BUix2&X-Amz-Signature=d3dda9266a8a2f4ce4e917dac27c23e97407a9a019e8c3508f392e5b0bef7414&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

