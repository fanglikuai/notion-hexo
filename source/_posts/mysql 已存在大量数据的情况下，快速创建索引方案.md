---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYSBUIHK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCmqfs30L%2B1OzyNXN7a5UfSvBQlh81u8%2FrPoOe3CSx2swIgDWaUcsH8cQUCA5wntR%2BNaa6aWY4W75DdOOFfNqppyWYqiAQI1f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIeN2naGipNFPWfw1SrcA4jH3r3GEdxGm7z7wi5QeoyKVGVfWjIZv0KpEJCfJw7IfDvuUoyah5dwrS%2BjU1gfCWXjJd89Nk30vSsNcl%2FdRKIGajDRU8GFnB09A2mK4xRK9dEzGq8Skgt42d3nP7pkHE%2Fedx0tC6AcGnjiu2NpoQMKGnulyTp5BzAAkE%2Fj84HYUg9WapY0KVrSeJCVUYQAECzSomAGOP%2BXpAJPxLmPMBCNF8jDTZLdeKiDIwihfNkT35iMbm%2FGRieeSZBJa3S7HENAkXSQyisEcbHPTxZyvaO8v3FrvGiAaYsdPe2picOmrGEjV72rjlC2hndLN09WnyU8gXYiulA1Pyden7KjzYW1dAc9VzsrsY3M9vDk8%2B3v%2BFZARQ4cEKF2rFx3AieDIKHb6ejO2RgwhW4Rv3Eb4IzhzQp1aDxp%2B4b5mrw%2BlO2NxVsGbIJE5HGtWsMhuHvobibKW9%2BNjFG9op2qC%2BOOmVkRI96q7R%2Fpn82sFH%2BS3IaMU%2F5V%2Fugs4%2F0tb8uIQZmCpWTBArg4w8j6GgT2%2FjHddVYMpOADwdRiUg59R42Qc51yw8F5gNA5SGGwlfUmzdAuKeWlbGOudOqbRqsIpP4vlo4rxakMyEd0IA3un4BIVuw6J1kxavCBzCGkBpj9MM%2F29MgGOqUB0EAzaeSomyZWq1USelx494I22PxCsuXr5U%2BwAScahfuakWRp7ilncyt1GfYOPmC7kY9jhKUDUgrqugU3%2BNeIwPx1av5sfPz%2BsBGZIblp4k9Y9zzpaaMaldsDpCinIwgreNU6w8M%2BRMEyrovgj6cs5xmTSwSAi%2FPlPEtU6IsHyUC0qgueWtp8mEWUxkuhSmFIod1BftG1UsoHn9gh0X7Btzhh5noY&X-Amz-Signature=6c34477357ee8e6146156e8e73d61db03243ffe5c9f2c3022d3a811805cf7e94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

