---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJS22WSP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T230052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCRakER4Z7M4MhdsfwPr%2FfcoqeOjruEU%2FfqDMJ3r34OHgIgBLwwAgmn6hxPgC8SAvyzkqEMl2yXUcHuA%2B6spnESVNAqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMOvtZgwARLIBkvqZCrcA7htZn0Epp1r5JpDXGgvQpcoooRLsU%2BYHsdvNwX6Obi3WVJM%2BYtxp8EihHrJBe4psoo8uBPnWieW3lb%2B78EU2mDgUwuD%2Fv0M2DshBkeTbeOkG7QVwa9HtYbh9JmHdSozA5VhghdKg3rj%2FfBWYJN6XQqgK6dAJ9TEO7y4ockD6XIuw%2BGkKyx3iZS0IFj93V%2F8ZKF98iJqVbptWPIicsLlXPcKs592UcW%2FLPhRFRq2jJe0SmxgE%2FXhyOTAq5%2B3FmigpnUDv2afWz6wpRIWN%2BXhY%2BP9iApkZeNwfMRvDgVtTjQqyG3MpsG1MPohTS6fldjx5Hn3RKaqhN7fPDbgGzn904KHXSubjzbHnquVcPVyy4jlKWs2sGp18l3AsuS0tc1tUhreikzwknv3yW1lx1T17mv5WKy102DbtySyO3NaKWGX6TQQtIjllXAiptYfk694pTXALjSkNcNtsLVo4S6mjA9BKFi3v0i6aZTHvKHTZt1%2BQYld9tnCxr%2B%2B8x2%2Flig%2Fyl3axb60iaNkg2GvJAehp09%2BMQ0tGxaO3UEFJGCjkIDppyigLyAlnjpRq90UDv9ETEpK8lGenEK2Wyphn%2B%2BNsp0Dq8wMS4rbxDYHkZ4Jp4GpAo9nMKIINpeWYeXkMPno2scGOqUBQgxwvl2rbOzirULFmrHTZE76rT1eEvfSnejYoPZ6YosaON%2Fl4kQF2rD3ui4qwbbLYFRVcRAOB4mGUGUoww5%2BKVuG%2BKcr6qhprk8aNyqeVkimFs3TfqaKG%2FeTxRhwzP0vg3sgdmPRHzypkPQZ5ZYD6s2Gpr5%2BKz6LSFsqUoLuoRZiRD%2BFRho3EcGr2ugRInce12gpikubq4x5qVTQJfuD5kXsdiMv&X-Amz-Signature=672ec9ec2c3bc62fddf317284d61d59d2f0ed78ea143003610b4fb032a15f01d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

