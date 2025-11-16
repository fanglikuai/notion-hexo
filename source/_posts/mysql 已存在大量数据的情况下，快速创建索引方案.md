---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVNH72DJ%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T160101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCmLGrjLoCb5PAdIyTvRlUwI0qFn7LgFEDL%2BcAjyVQ4mAIhAPZeXVF9nO52bpdvtkDW7z%2BYKVWBTvzomajdDMM8YlyUKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwecammCscfy6VKCqEq3APBgeyv6V5gBjmq1C8OI81uuxtplUzGUakl7gX4D93OwRrqpr8N0oiRPAhQO%2BkVWGgK8EV%2BLtCWyNdU90Zs1hnEN5ltJpiMAMD7n8h9Xlwbq2lB2JsbW5dqw%2B0DS7NTrgAMoE8N150Cw%2FjepC7wnBNdHv0cr4L24hrRTlC9gVnzQCnwfLNnEt24hFzWk2AM9RYF2NfeBib9FrAqJfowuY6Op%2BiooU5lfY5T0KDnKpnLZ5g5Tf5O%2Bk3zpFgkKGzRn79Q2W1z3h%2F1bzmJVTIkVOsGSTkYEfCPZRk6JsUZ9giTzzGGFndbFy8SSG%2BgzDIwHMSbWwt2p06lE94RpQFph10RVj47n%2F9ZZAOTGr7EgcnQSWWcnauNkp41eLIFAZ%2B8c4%2FsXOA%2BYtJ6h0ujuWzoRPPVu3GFWtcK%2FscqVirT87qbWbpjc6V6T900i9Stb13TtRICx%2B4Srac5JsjNGXly2klFjetwAH3KPtjX6VP7tmILzwRTMZ26VDriRxO2M%2F1%2FbrImXk9SQiqyd2BAkQGEOkFZw%2Fe6hNSaT8H4Phgcgaft5WxRPa5QItamur%2FBmLlFSmES3qcNA%2BzuPT6%2ByuIdHaW9%2Bl3JMAIugkwyvT8zsPE4%2Bqg9oVw4G3HK0rA1GDC63ufIBjqkAfz%2BimWGB4AKyaOvlrWnbDp7Jl%2BQCIKwXEMEHsLIN3wKcas%2BrkxkUMYpSzO9D%2BngR%2FzVpiWe%2Bz8Mk%2FfkODDLZTQ%2F%2FXEQhZHP3VIZoSCBru34sXfE4zFM%2BoOMmS%2Fl7eHM6B%2FVJaKk0u2C%2BEpT25aoGK1H8X%2FaNiOWlQeU9F3G9z6LX5SPFkuACAArqm01UiUHJgwf%2FG6rHIdtiTI7KnrQElT7eFdG&X-Amz-Signature=ae70b1b14f4bcab01f7603483d6485368d0bd56bc1464ee3f280a29e1580ca95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

