---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUG4XCCF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIB4cBUcrJn%2BbpAKyoy80ofy2TKGmSnLq1KPqc71AwgFIAiB6XHgHpNVqTfG1JpIkb9623XRD1BOEzcgwVJUt04KAPiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDTVaYNEtgAWv60qgKtwD07Lhcj42XF%2Bt41WkoT5MJbzPg9zfxFh0buFcSCqFQjj9NQ62GXBNqVSLEDt7HSs%2Bq1PFBniIdFDVA2H95yqB8SSvuCmrRoP0gf56XrhXkRgU8Y3NAjrrKsQHefGlGic2cBHovrkCdFjKA8lh%2FpZI%2B2NnkXHVjSP1lxbHR1nQP7ShPbvUXCgpFieBSD%2BqtKaFzl3A06CffDL7OMzawHFpWUR5%2B%2FdqWhtDd6M8KIWQGhIR1X%2F7k1%2BqGo6AI9t5WJW0D%2BFiKWEQwJ9SVAz652d4ntcuKUDiOg2eOCLPOIHZCABn3iFs6fPXiXS513p3gFK%2BvpSW31aCd7nTVwuf56hUyO81f3klBD2aC%2BIVMuQja4Ae8GhikW8RXt%2FaqqO6mrLJTU3CySixGNFf4367XfaTEfnCSs1A8LCqO3ywmGHkUTLxcl%2B%2FrYj8e2Vo3GATbWRpcLDrgth11JSoz3w31xJ2NK%2F9cjPrESataadPX7BPLHLnpNwTILnrlwVoW%2FqfvLEQcdhvFx9qepUKEm4v01yJY3YWYTL09ZDfG1v73uEQQkeLY4oNpZwK2UB%2BW1W00u64X%2Fx9D48Ljxc6HTuK6zimFov2nEr8tRKQOsTqyXd%2Ft%2B8YkfPHy8i7gQaXb2cw7JHpxgY6pgF17G%2B0D9czaHOtMRSavywcb%2B2aWgRQXFeP0Gfrj2E65mhrFkmLzezAf9%2FxRsDQZ5wRk5zncaQA9SP9Yr46eyluu6BaIzgam%2FZXZ3yIt3ug1T3xXOuvL0Ud%2BeWF0d77z6T%2FlNx8mvYSt7h%2BHkDR%2BPO8VpCLrHb1PBKeb1bUybSEmqK1zq837BO4nwxk53LaryeVv1zqOz51zTUsTOLqqvmv5G5wWeDg&X-Amz-Signature=956a8f68b5e0e863c77fafac2345105be23a0c208afc8c5b14566284b3073edd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

