---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZA7BIIW%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIGYjEW57zbagRZjpjA4y%2BPbyK0rSFLd3EGYU7cA071SWAiEAnkI86fwh2WVHZIC7F7GhLPJO5E8OTMubuMAHej0H5EQqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2BtfZeH5BG2CL%2B4WSrcA70xYnS7pWuK063mis5Wkx24IFfgl5HmPgR0UISGv%2BdX%2Fp2FgchNmAk9opRr%2FS83tBoPX0U8onAlBfefF4ltAKgo7uyPQI4iqNIS1oBDmZSwGLbLehB%2BEfxaC1Rf24RUoudeqbYqPuS6L09d0bmlUbkn8%2FEqSYX%2FKjFURG8b7IGGqpKFKnjCu%2FnVMvCdjr0g5AKJiYcFJRPD6cwugMBK3pr2ykwBtAWRR6giOh%2FU2O3VjhX4cT4mxM1NXzZU34gc7ZeWKjqGhn4ckULSxLFOfwkk7gWUJniKNlSFnrDxUmScQb60lMH6RlED4ZxoSHMZIAwuytZTiJfEdeWCulWHqzIiuWtVxt1L51000LaNDIQn6matkH%2F8F2%2F%2B15L%2FAJwYOWRwyukcw9tLF406JL4qsCDm%2B5zfuXhQty%2F11s6wyybpY8uNpxnQHR6P6EakjrLX7KDIw1cwGxvK12kda%2FTmIqQjgxmINlIFxxUY%2FICVyeKRaIVY2XsfQHsskCEeR2WUvH1w8ERgTVNtc6KGLWb0fBqYPTx%2B1%2Fw2Gj3cbM50%2FirEJOiN9kzaGlKyzb1YwoYJXlglfH2jNcg8TEgsNgCgoPFgqdfLY1kC4GBvDJrEMkU97rjh5y%2B3fK0udwQ0MK%2BN3McGOqUB6vXfET5Yv21P3%2BqmQWjIOdqDUmcxBWSCtqah62u4CeM9He5QLY0f0lm1jgSW7wAgm0GjOcIIwlFhDM8RqaHmVdvDvjNhL4HlBw1h9IALYCeCUuQKXW%2FH%2FC8HU1N62LAd%2BCzZBE0bEFoQsMSR76DxLFmk8I1guWAbSk8TmYH98wcq%2BmiM%2FGd9%2BbUnQZHrZ3RqD3en%2Fu0Fe%2FQpU7onervJgX1U9Jy%2B&X-Amz-Signature=b1869962851ee5e9fa79cb895b24be05709588ea08bf3afd8a93e2fb873ec31d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

