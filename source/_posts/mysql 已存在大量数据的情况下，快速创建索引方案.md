---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QUZBI75S%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH5fwqnEgSPaQpX2bmO7G2CuNHTALKy4gYNC4%2BUexK5UAiEApnSerlQjLkanzSxe7Z7Oz1W2yOmU8Q17J%2F3hD9GEFtQq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDP%2FamuoBrmdYD94tkSrcA21D0mUWe7sT4VYPh0ymZ51PLH7WCtB8J2pAp%2F2mKmx0YtlPAtwSVCZ5zyIyldVKEYgugS0fgOPUUC9QonSUi8SnFy7%2BzpsDXevf5md7vydeOGfaZZfRupaiMQjH5Pd%2BYqxEYh2HePPQgGF3jiFqpBboyr4hJbMDKrs1PAkhLGXY84wkAy65ZZyP%2BnZrjom86C0ZoeJSEOepHS%2FnNbCnw4w%2FgjE1moEb4y2LoOaSdkVEmADF0ffsUM1oF%2B7N3C14XkY8V9CeIjd9IFz33LOnNQ7Bcv%2BqCbTRUZtwzImhbAcJh%2FSHWeTM3uvhHxaPSumS8%2FmcXUtPoIUZNU5WEQSs%2F4apeVAWD3Wfpr%2FYZ7LoNmNzBqt%2Ba8w0tbn97WNHVEtPk7565ljODA1VO6rG2Zlv5%2BdUKFiHFcy7smUEk8kdkgGlFd2ZsQcgDUznc34KsX8k37%2FpSVA553m0nkmSS5FFyMgASM62r8jHDIyrsgFuDjv2DcUHk2WlKFK4epRLvhK1mBGKBMzqXVnOsTZMq4Yb7KtlRh6aP%2FDFS0Bxp%2BFNcMKLXReb%2BrxCFARcYazJdjieEJhyRamms81efXbwNV5foN5B1p20u1mzfcG7GomDe%2FY8M4MgNsyd%2B7jyyM3kMOjnqcgGOqUB096di2v2EFVe5hx%2B5UCpc3BqNzwq7h96IwIOE7bWQxwEXeuptEYkhYY9uIhQtdRbOWD%2FgTM3LLurcL%2F4KsFowi0%2F4sggQYKFyrLIRQK0BUS52cc9LFR%2BW9mfEpOLINOUQSpSiWpiOk3d2hEw%2BB7BeXXtKn5h74D8Nkt2j3ejV1i0ROQNcSfQ7NxCGMOsPf4FzC%2B5tEAW6TZ0QN4y0BZmM73XwbXx&X-Amz-Signature=e353ba3316261aa74b8704db01f6a8a0e873c3dc4f77a32324d8d429186d6b19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

