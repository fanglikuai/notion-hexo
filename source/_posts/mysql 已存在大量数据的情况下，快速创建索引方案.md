---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVZS26XH%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T080105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNmdvE8%2BUY%2FU4Aminu%2BWpdgb%2FOw%2BZ5bVKcwwLZFXSyqAiEAkp59CJGq9qpvaPnqKp4D6Goh0ltMipImsV5UmAw%2BaEAq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDCCQDJWo8jmtsmNrYircA3YEqbUBNQ9VlBM0IFEEO%2BbxK6AdZG%2BOszzNAJPnbQX12bULP2spBp291PFwdu4UsRIopkeTfAnLzQyT4KWTDNeVtx3MhuIpGL72kY2BAVtsfXSdrNjpZnVD0DitrDOCy1MfSlTbi8Nd%2FT5i%2Ba7JMVzKuJtOihrzW1xmjRjb1ANBheCMGQL1HyALqEG0ezuE8GJEHMSo6AxpiCqndq%2Ff3%2FTzrvgQ%2FnH6j3D0zLDC4TBvje6z6XC%2F5BToskA%2BhaCPj6eY8LvQMZyXONibSqXBK7z3AF80tvfmTzyMxjJngYJwyz4MUhzpCx0xYT2kn%2BWZjWBTAHoyn7YLD7Jz7V2xTREyYHntuUcb5KqocJ8RKGeIvSQc2IWPKSosKx0A18KxI11GClPOzE%2BCXAL5j3Mgyo3YB%2F7gPgYBa1%2BqcmexXJeqZGdJrvUZrycdwMh47aE6zpN4%2BX5aPqIyco2c8rHGbnyqpKcoVOZ5bhN4MPPeukou0DGhdduA5jQRak%2F29DgX4JOOcA5I3d2NeQMI03PNWSEEOJUPDGSkG6x5%2BrapldcoFg%2F%2B23orTg7SUA0h4sNVtyRxXmSBi1quCZl7lfkle%2BLgUUJW2TH3opWUAaEW9NqGId2caDuxGv%2F89%2BocMMq1zsYGOqUBfH8NGXmS1TPQ4h%2BfQZesEhOssf17WC4WD8NbxWijhZFVCwwzlpa3l853xCqC12kdjqgqhYHxQ7azhr2Ou8apbMsIYh3XxT%2FZpXQ1t8y36DsMQg4cU9AmQq0SJeUlspEV%2FyPzx92HJem%2B5azQcAP%2BklsrdxRQT7RCGIOjZWzZlIBkzyZeIPhxuj6XwDWMsfX85yp%2FKAY6JZNqot%2BMlxV1gBjQ%2Bupw&X-Amz-Signature=2055cfdede233b1c5e7fd099e6bbbeb85102879e63928c06d3c209f28f860b7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

