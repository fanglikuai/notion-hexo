---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVUN7G7X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOswfpNhUXYoLUN2x7HhP%2BFr2d%2FiuZB2yKm3n7YflXygIgEqD7cfpT%2BUu07xc1vDDoEVziJdITXu178bAMLmYCmygqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA6Ib5cc%2F2EFPhBbqyrcAxWKJAFWFz0rBQl%2BadK30Lvw9kdrT%2BIRwr6SqoUqbPU5rq21cxxwv6L2Djg5pgy8IvtDPqNmAyK84NDPQs%2B%2BVV47UqafuM%2FURLfMgoUkaE0c%2F66V12WJtWOGVBGBdc88UwOUJQ6g%2FTMIxYOzMO2AT4bjoP9cD1AAfHrAG3T8SgmGGRjqZ8uB33PtpdIgDuiRtDfSPFVKtA0s31zWEalQdRwxyS2eBR8GLc%2B3X7iF1Fd1bx9k7hAkbq4IywRnvqge5VguO7ZgFoOAX0f%2F9k8OPmdjaM5E0Q%2BI61tTgpWU4nLrknN%2BL9HZ%2BUd4doDPKVcJqTrwCqCn1Xl9c9viiU0D1qe5XqnC2PyktdEIk2dpPJRpA0EKfrPPKeT70OsmwpO3dOYItSzdYbG4Pag1dWqedtiG2B72CEQzO3GrBqQO%2BD1gejNjksnGsGy0LtTNr52VZLbvJFMK7wDL6UNmAatM5TXfkCaLUV8PmmqqvaDStYjuQ2Zo%2BsVPCvPUWZVgA%2BvnYr9ak2%2FNifH8w2LS813TV7ad8VEA6Zptf7JwXs5eBaqEtTObDpScBlFFA6robRbmNK03Ugi8qR%2BgqBKI6Fh4UzTlfMXTrUHcH36E36XnIoavYnGyPV7v6A1G2aiYMKja%2BscGOqUB9RzySpvxwdpVblIQXXDpwUqPrqq8scxgGyonjoCl%2F4jT5IwdAY7xe3z3TUSr1zDo6%2Fv1VKvNviWS%2BR7vehj4ub8w2Uy4AVkM3Xfxnx%2FV%2B0o3ER%2BTAUKa1NRVYsAoDc1XuPPh8zSe6gZqm1oIOwChYpcKa4Umvg9r0jIO67e%2BFMtKkEkIA1Ni5VQNd76gZiJkUokNLpB%2F2yQ80UA%2F3%2BhtB6poflD9&X-Amz-Signature=fa51603587dc43b01ef233adcae29a59688f3695f70e8701a568a8c4a06bb534&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

