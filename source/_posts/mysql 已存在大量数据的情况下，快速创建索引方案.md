---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAQDN4BD%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T050105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBRQ7kPsdf3d5VTWYgSSMVDi429pHrRgkHGIrOnLZ2JPAiB60qM1Veo1ejKRCPVW115RkMnxzo5smjTStfdJ%2BK6WNiqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYy7b%2FfuRf00hkvDvKtwDuGP2Y0Jbo37Uky0pAcbPdGbFd6oGcegMben0bdp2lT%2F3njKnEUE5q54CHBs7LDXuS3od1tJ2QHkJwrmpyaV%2FkgwIX1uKxbVpoarVCOOhHA2XhG7GH7OmeCfVV%2FYhh2LPjueFtY6pv%2B0wmzOPRlPC6rqGKJtigc5gNpcbLJ6Xb5rII9EP2%2BtH7hqm1LUcKG1%2FJm1uA6Tb%2B6WgfHWXZAxRPq5r7i7hWBsHn1j0nOammitnl0H8Pxe9%2B3hi5gsW82d7JFQjy%2B%2ByGNeUMhuzmIoxwl%2FZgvaQwScwGZQoDpEOie3nv6R9l%2BeVN9TwIJRQGeNSgRGzk%2FpidvHZqwlSNPeVT8ro1m0m1%2Fh62DVl5HGkbUoned43v0KiZGc1qmon9s9bo%2FDwY8Tr6V%2FvDpj3EWycBBJB5Vk0amZpwnJAheZV3bnupZ4S4NpRwyC%2B7I34LOmirN0IJTzLM9JG5SPWZ%2BoAuNy98CPoo2%2F%2B1S3L74JtG7z0oY7xBqSAc6gQ0iRz3%2BbIJEA4M6qza6aEHxDy8VSl%2Bds7PamRCvpy6fOHW25Ex9AJNy%2BXMcq7%2BmDW4V7hjDG9xzztlrU8iNEd1%2BsqfrYDU2Wx1z1K1bou3vHIfYD%2B3eguJ0Ju0ZPFL%2BktZoww2rieyQY6pgGnJ%2FEwUFBRUsla1IQ%2BHH%2BRwSCNC4HHc2rK4%2FHhkUS%2ByAMyJY9z8%2BDPT1hXp4xWeroeGPzD%2BeTStSEbqUmf3JXaKPFrKrei1gH3vCI6y5ITOe4gP2kvHlYK8g9cnF8R4CT0Yp5P%2BrTS0IMBe%2FtvJFSKpj7%2BrPe%2F9Yz8V4AWYzgdKM2iV8MzrfQwe%2FqSu3aGrqHbf%2FeDQtX%2Bu92dEnFWAzNg1lNFtJMp&X-Amz-Signature=b96b22fc8e8f7058653720afcb23e662ee9636f2f2552135018ee26545072daf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

