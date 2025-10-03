---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTLAAY3L%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC0CKMGOy2oCdC165xfiXfkBwkrQHoVBPbSpY%2B%2B%2BO%2FmhAIgSHOVhDZ9BcHcpbB6CidwKcVbyohCcCsAwbeQ%2BOHc1%2F0q%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDGEgaYZl%2FVK%2BDD1YySrcAwzj7CGFhO5WK2pmUmi1Jla646MFegnGZnSA%2Fywoh%2F6mvbWpuJwh2QGM6cMk107YL3GCSKVjDTTtBcD2GNvbQ6NtPb4Hh%2F9SKA1t9aTlN9W7ileGuaNLBRS%2FwgdVi%2FqjIwTglpclWUJIieXoTmyuUyO1nmig2RtD6a7Og2uHnydG3Ddw4Q2iDI3xhNba4qyWD8QWQlkjNsjOvM%2BhKqUZI%2Fml3kpPYLJVjqdZcynJGkb69i6fCxjSsIPnSBNrIqt6YZFacWM0ScE2QNeYoxKzYX%2F47ghitVcJ8FD%2Bq7Gb43mKSsCuy4zHrU49IYxffLpYPXeH0xxP1Yn5JaR3k%2Fabdw1SPd1yIaWoaE0sZFLUbJrpjwI5PdapbTo3ExpcbR9iPlQghiOy7rt1RoWvQhN2doaU22pWKmgJrMEEKlvl29lPJ%2FF6W%2BKEc49%2BONuzP%2BOPKFixp8qkpOCyJ20Lq4fE5up3a3qz0iowNapjoySExrBdDZytSeEYd6VT9PPWF9yA7SS5uPGuHm5fekH%2BR3TD2nRIbKuZZF6HcZgzU4bklVweaw8sKAUmf2uS0Gz6mALl08Jlx9Ijdf8IqF9xXGtN1yj7q%2B%2BEbkfp%2BFLXkMvON42Fzl070DEUa9fZROZMMJnagMcGOqUBAgU4lqX5ZUDvPTponGdnX2LHXThaYt%2BVMJN2ePos1LvCyfmwqd4eUW69y6oMQEfrBUnL3pnbIyKIiW4rFlS4Szi2HSuItlJtmnbs4FgCSehlH%2F3%2FSM6xkhi6D5iRXg%2F3uPHD1OmoNL1hWMPc0IxK5VlIBEha0OvOAfEUEzsni5Krwk50zj%2BZPpEseQfsXLuJIdhy5nECSbT62dQDq87QTRBf7K9o&X-Amz-Signature=c539dcb7c779c8e5c0e1f88b029c51b362adbaa65c2e9ce18eca6bc7d835a323&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

