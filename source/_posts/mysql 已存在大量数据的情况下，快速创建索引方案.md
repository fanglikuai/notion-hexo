---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674ZZ6J2W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICjfWpfZ45bNaqu%2FMVQwT%2Bb3GH%2BsRzVsaEW94qRVtI5mAiAs3Gc7L5EwxEbR0%2F9N%2BWsf5GKPujHwyVJegIhiqcUo6SqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOsIG0A%2F%2FxO0l8L5zKtwDrZ0AwYLQViaNlIPs2%2Fs9KOwzn8y1f2Xm9cZ5JWuB7KKR1XowcE8iJVS0uBsDiYzzq%2Btx3bPq2dMKbXW%2FR3Ch4HtrGlhij7SKOp%2F4BzcYrmdgAYAxSummWmDT77tkGtx%2BpMnsMy6H9H3ATkjmtah3h%2FF61RhvGr%2FBQEX6YMDiKsmG9cZPz0o7NDZLYG7W2wq%2F%2B9Rgd5nxroDlofPVCxn7KWTi%2B3zNjoLJykEm2ElobIyCRtKo0Ezt2gpTPXPR8L1C8R50%2BOKpK0hX0pSuE4ZOORzjugNniBgIFpKlikZaKVRRhCxRe65CG4buwz7w%2BoEpIdS9Z%2FO0IXPbZ1HHInphdrZeNta%2F0MIA7ZY45rC11fn5%2B3YvqphY04Pg%2Fkygk5sCtpeqtat8KOb1uKWPJ2Oy5uDtNF4zlU3d%2B0V2jVHTGVBWJWvrVFLhMXGhhynxumTdS0%2BcsXLmB6ejcN%2B6WHegCA8Sl2%2BcCnyv0CI1FEHn8by2DXFkS8MSyl2PrULXUFDe%2B%2B4%2F6greg%2FzTjlY4tEs0g8Wzp1848KnmT91EdZ9XdjXMZ1%2Fo1Fypxr1U6eupX5vvJVzhRR4zudVr02BeSB4daeE9H5vD8hjPxAlVb1bbj7G4h9PyISZraB%2BiHUowhYD3xwY6pgHzLeuUvb1eate741xfd0KNMd7vvvvdDU%2FsGd%2BVjTijurh0jesG6Ba9rqbHT80F18PB0yg8a1RMGm4vgtzk0xte0ezMq9rT1LxplMf2pZ3wsMePb7Ga8%2B6QIh1TNa51nMz9oL3xWt8pUoXlR0kGuFovjUiaX3Sxf%2BGw7vvTNnEjsMKHqgRqC%2Fm9ZHuy3hShi%2B8pK8DlAUvNqQnZNQsILb9spFL%2F9ozY&X-Amz-Signature=9f23b07a7a1e38c160e75b40d3fc9140ccf6e8b2d60e969333d86a2c157c7d37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

