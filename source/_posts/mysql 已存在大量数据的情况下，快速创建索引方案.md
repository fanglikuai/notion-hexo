---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V74R6XU2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCvn6GiX4QA4QfCZTSSZc6Dq7Ih8WtsCcpzbYtDZd3ADwIhAMSgSeqXL2CPQ1538OWszclZ28AL43uucko%2FWXtEAlCVKv8DCGYQABoMNjM3NDIzMTgzODA1IgzT1wtEAS6PIDH5x7wq3AMIMHgr00Mi5COxhvYyfYAr7qZvbEg6Ki2KdXsiIFe56eL07WZ7ZKEzOnYSn9hJN1gM%2BBdertIXuFso4RZ7ziWPo20NI1bnJp0et7J8cGLdiGVJOd1YtLe2D0xuD8OZFnpZEGfroT9aanoeZAUbGMeBQlCyl3GhzZkQ1BBOCZtdCZSBT2JDptxti4Iicozy3IGavEGe4YMsItaxiXD1bDSZImBUPxOgWUuArQpDu%2BDFBbOYovd5QDytwddKOkHYGOrObBSCrqj%2BXDJC9jUsnw4BCO5%2BcCDF5BZQtGl4yUaEd9k1hTTjsm%2BSSZ4vXxs4vIW8JISqZoDaCAUM6dtIEIFHX99qu%2Ftp8SS%2FwOdCgDr7xJhTAVfO6fAnwxoLcb%2BFBK7aN0%2FW5v1WImGUbd4cbAQWdbeOrCNaga8ycZEGz0sbZpMRA0wHx8YfK%2FGVqsdTo6f629lu7fH47q3rufYJPTKT6F5qbfSwItS%2FOS5QH%2FsJFxkZMcVcTYgFoHMFnOGftOdJrKrZzdRM%2FjKsp5F4gMqq4ujkhS1ZW45HY4TlIko6IKIu2EYR%2FWY%2BI02gH0D1M9TE3tXr6wCVKAxumOQjVOXB%2FteKc7y2Mr3jpFOdPMORa5stbQWb%2B2WCN%2BK0rTDW47rHBjqkAdFJ6sdbtumu46zn4u%2BYK3pNfLzhXWb37yKHSRFL%2BH%2Fh4RDqP43VChCBdfdQwy%2BiO1kgrlherA%2FRusiuznpHWFnGsPRnO4StXy4cYlI2CgAV%2BWiB0U6QEcAX6HxC5dpFtsmfAtj0INXXHKOaOmpmAMFYFXY6kyA%2B3jeCR%2F99NAmjnZ0dkq%2FQ8eKYxnyZYuSUuFcdDgVYXoH%2By81eQLptQR6R7jjB&X-Amz-Signature=3fc543175897a801c0a31b749c96f38f6b3fb0e29033c9e24e20653cbf7cc5ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

