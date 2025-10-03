---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SU2GOCTV%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqhl83%2FjoO%2BJ%2BVWhgURuqCcUUYMcaHsooh%2BIX9PkQnFAiEAsDdipTvNeM2KSL%2FMmPuI0PrrJzIbehqKlsPq0NEqsQQq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDEVzYSNpkn0OEkwlyyrcAz%2FhwOkBwZdNeIgYCg311qtM%2BSDZOV0cZw7nzAQt13MmHC8wYTIff2WaXBsT%2BEgD5ygWlmpzr2ZotiTCSuqlJF5UXILaF0BSrVS5qX%2BuWINVEZ8gqxs7CuH82xaKvfbSkY5EgWUaJLjLOjy1y1wWEkZOH%2BrYBAZHGVq%2Fr0rpU09LfX1GaGZsQJwTTrANlmLSCyxyVJLbaWXcED%2BTfnYDu4ebOibuvlclVefry1ttHDBFTn2pepPxd%2FBv%2FgwylipdN6JoOfDtBM3yQ%2BSu6kTruU2tegUjWQ10Z70DdkacB4eBLvHg3TlJ5D4kluvRtvFAYMGxG7Yx5kfYXWqrRzhqNbxWQDvoIgOO%2BhS1QmA%2FBh7JodM1ODGcF%2FsDdNULXAkEtSsmct%2FhLrHG%2Fjpa6SKlulVxNZ1uDgrwLVkuPY4KcpI%2F%2BQUVUZlzC4lL51EOmqI%2B%2F4sw5%2BdXBsZltCr4VPKXWIu8fGKmOsaQ3jeZrRhOMdm4vDkniIx0oS22dFv7qJdHK5w1SaNy%2FyDMeNGIu45tG12Roh0d4%2FUR7sfqbESA37%2F%2BIA8Vm3SjWlUl3T61ckU8tirKgvoeV5FV3jp3PhSSmlMHYyfwLP20znQCSnA8q7f1CJuMZTS%2BmX2X2PGNMPu4gMcGOqUBqeLahfVivbpolElVQ3T45cf0e%2Bs0Kg7%2BMrS2hAdxaI1lHH296KcY8oCj7o9lmufHrFQoHmncBZ63X1La3ViJ5QoKxacvGAX1YhVjOLwFuebX3TqWl4e3Z9iiRqEVvU2vB16RYedJtoqI8u67RmC1gIWh%2B%2BXD8dvRvewxnpiuqDDyTI1qrC6998JXnL1P3vgOm0a1Y8Swi3xa3cUuBA3dU9FJfuaw&X-Amz-Signature=c4d97439209fa1dee10db7bc21a8fa83639618e0cce87d3181c1dfa44ca00b0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

