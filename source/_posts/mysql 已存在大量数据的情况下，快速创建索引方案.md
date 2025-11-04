---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THIVCJ4D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtclKrU8SvaMU9M00jjzFOXRpFudG1F8OgrAs04iy83QIhAMOFLCYPh2Z%2FmAn%2BY6lssMPQcTNgs7SpyrXPRIUpfK89Kv8DCHUQABoMNjM3NDIzMTgzODA1IgzrUzLzUAK6at6yK5Qq3AOPVCSiXmhEBCSzA4v8m%2BFrSYXJpgtgSc%2BDKkXP6BMD1bkw6h4XSQhdys%2Bzx1Z8JEDbVcMoWLtaWPtEgg3NDU4M1ajxtROocCLnf6%2Fl8fA7CayDheXE1cqy3FWI%2B6gHOESZMWrniV2JRRpRjVCLOf%2B%2B1X5OftHrYP%2FECHd9O7ZYbNJMq1FFt2EWzGWIHM2nXtc9eBXdAH9gqY3ySGZJZrm5nKZk4OtjLGilU18dAJlTyZLUC2FBdhv6Q7IQ1bYL7c4LuW5pdSoEFyDbPIcEAGc3dZiEM5%2BQR7IuNVeSrPtkkHihbMdTtN%2B3hUUAH74q%2FQyj%2FrEzAB2mn4krPu5nxRS4JylSb64Xb783q%2BLTBE0DIFKnGBeadNeZ8zuUAeyEqLswXmCEDEksagVSIRLqv5T032jUSgYqbrgK6I53mxgsueEGtcNf8hibcJc1weKnMCUqZASooekjgCjWjKRVbxKE1CZSeJ4xHEs%2B%2BDTgZ7DiRqVLupaJHLJhFHVP2YMe1uz0R3ZemOrNR6%2BqoG%2FYHHufqvBoWkK8%2FKNqHa1DrAm5UHg9Ve0qp0vlDFBLvEqWobDPAeUd3T0X6elSQBiOLpEPREFJRU%2Fp9gAz4bE6JIEM06qiA9nNxUJURwQCGDCT0afIBjqkAaUGP%2FkdKT9TwDvo8ZG2hCiZRgfM1tVlep1u4093WPOFi6RlOvqBdYtXA4k1Gbmio4Z53JymAV0JJPJRM3VsSxIdH2FuMS58yarMpqK%2FIRosDNwd%2BiCC%2BgbOnDKocqCYG%2FXSnqek%2BqXd3aOrEv1uZZvYwLhHK5MsWj8G%2Blx7Ad%2BLxKrJFoI0ggEnV7SmiEJFJ7AajexMX3PBIB0xtZj0o8vS0NGb&X-Amz-Signature=454d8e5a6b78048816537938da720b8d88787d4bd88f638f2258c9ebd80f18be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

