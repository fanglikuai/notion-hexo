---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDCTM4H6%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEG%2BEuw9h1RXtV%2FUw4hQ7LVNt7rEiZ26g%2FhsrZWkpgN1AiBjdh7xZIf%2BHRtQki7oyeV0GxTR4F2Hn%2FBWPz946sK1CyqIBAi3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXfRb%2FVrvfjqYZmOKKtwDcUMYBf3mmbP3dnp031zSYeNsEYYuVbru60sbudOgdqCyYpsXO45Pv9UnClKXx6wjpwKd8IYwkKu5jVYPDtKeIKTDLpfj2ws5%2F%2Bb260gJ%2F%2BowMDPDJ81o47RvywCicgEfNMPte1bsx8Z7r2jVYczK1%2FRYtXObZqjtIccoIyYHVbcX2Ir3yel3bxK7Y2cID%2BY8KDE6uyNRVEgssyhWDyxrdzs46SY8kQVJUn4y6J%2FxNMEOKPquqx%2BFm61hrlBvp3rs7dcrHK%2FpEbPHxkNI0sIE8mse4AUBitEXR3nNshxaYNBqebIkK%2FtdnzTf5TIbiQMYcOud%2B9tXne0C%2BrXstA567tDU0%2FdZpJ0vLpVjjoKuNHz45NCrFjVF31fisJNu%2B%2FyDr8Z6EfeYlAYPAMdD6IJfxBdUJrZUJ8zeTuCcfpm%2BcNtVtF%2BNz%2B%2BkVG0JKBKBZU7vSJz%2FMUHu8gnKHds1iDSdqfv%2BENlGkBPBhZKM05RQyLYQfhW6i2eEoeBXdUqsT7Zp67gH1iw3NOsDQqAGNir0PkStI8baxeKjNjgwmdQ3tvsGpjKqUnOQP6H2Oan6rbtAWaJnbbtLR4HTyBsK8guNxBQa0aGtdvni1tWypVEi4SuFWSQkCM7XRbH6pP4wrbzuyAY6pgEP0ToI%2BkLzYelyo8lIG57qSdoWnoziTAGlQH%2Fu4xkiruMF1M62u3BDDssjxfe0Z%2FnX4g4wjxRRbtIMZko1WlPBLR2fUDEBK9b3O8IOiXTNpCrNadvQVVoAODGG66luy7070vUtGVHMcOXjaA1J0F8%2Fq2xc82qeA6UxJVm9sy3w49oRObHVAWb2Mvq2lCx1preTtPGDcNH%2BApx2JD1QSUPqldFtqH07&X-Amz-Signature=276103308c5cd6a674fe2aa39458adf13a39301f812b2a4424b2279f349468a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

