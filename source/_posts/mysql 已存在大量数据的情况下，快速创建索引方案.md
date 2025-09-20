---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UGIZUSA%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCICHnfpqI21EgD3PQfh6FkSVXEmqfA0rPlveOFeSDlTbZAiAmBCOeEw%2FB5sWJEhYDybh8rYTtJHMzCfu%2F3%2BOa7dOqHiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAREGrXuvHIQMmoA5KtwDov87Sa3N4H7TBFoTuh8o6UX%2B4Pfro2JhansCFaIuLVms%2F4gSmOg00YP2Fj%2BEE2vDI%2FGetwFTpM8jf8eMCgPB683%2BthMnwaLkWCsNnFyNfp%2FR5tYzpFCF11Hmu7NQsu0CXjYP%2BIIV9%2Fv3ZRrb5ZGKmeWCjjMfqbosTR8VtSNmdnNRfSVOtFLI7WLFpYhITti%2Bm78ipkVcXxfEoDLNHMur%2B3%2B7uMVVwv4OrZYBRLFcwGWsYxx3EJNK%2BBzhZQkn3LuA9ZZ5LGE1gnCCFTLNlUuoQvYNGEIuhBtCueKWX1dMCxkHeI3rwOwjNpHOzi4v5%2FMMmoNEWcfEtNAbFfqCoH9Q76YSdesND0aKuj2uHFpQxdbyMcWJcQ65iKYJtaa18KFxYuYv%2B53rLV45b0WOOe2iqpXj%2FhmwE91RaPKZYHrmbyqoVEIbY%2BkuV4B2znp4fstOgzwEjtea5o8qi8J%2FxQt%2B1VHg%2FqOXjFWwnJRgDuGvmRxSRjEU5umYNrhXFjvX2cczv4bw4ebHo83x4J63HF4AEMAn1%2BDmdzMOM4hJax4PqMCBGMAadNf59PiUVZrIWE%2BHL7EYQvwDDnNpxQUX4P9pn%2FeKf7gf2y9n5wbYwOM3eXKKaTw%2BACE7kjxOxXwwi8y6xgY6pgGm0LbHrltlEdfQv3TsDFh1NliCSAxLh%2Fdwf9EAVdnmGtOuILDA%2Fn10xCiBpf8CClPctHB314lqGclXz88yR0Aa%2BkW%2BMlks88HzWROgrv2tUn0Le6xWFC68XBq0UdUDdB2whr2aVUs260ptkxMTQMGfkcaPLhaynAewvOfM%2Bwg2KT4trHWjkPY7bD6j%2F6eyr2cAyauykNBfvFbkiKcR5Ssx2uRVcfwO&X-Amz-Signature=96257ed9f47b368c6d4ea7e48412d6c04fc73bd492b2c7986f78277b33f595c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

