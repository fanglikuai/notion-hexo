---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQO7SB2X%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCk7ZYKtsuZALpxrSynEQx4MX2goaKip7u7BQb1jYUusgIhAJlh9JnLvjY8R%2FdEbqUYbdRRbJSKRELuh69YPEQQKYr7Kv8DCDUQABoMNjM3NDIzMTgzODA1IgyyxMaQ2rQbdtxz0z4q3ANDcbzaZxH%2BDL9Z8iAaE4X5uOfpO4jAZhWXlLWN9i5S1YeFTtehYLhwZufy9KoeBWqeLEz6wu6HFncBummYdjAj6iErZ%2B5F%2Bmbhhy%2BLGZDVMq1xh5y1DEx%2FQ0LfWZ1OMhCFao3z2iZu2rDJXx1kyLPHzHLv8dj4bN8Pqi4DM%2BtgHD2EaUTk4cOYeyv1%2Fvri%2BqtCiytOQkMYhA2lfF6NWYZ4Znkkf9PjH%2B%2FVVFRhOSZdcwXDk7VIFZwDYvJX0xsdIuQcdoJFKSDNEEcS7tFFHTc32ycfJGdvhDYCXXghtxAHYjKoFjOfyRLQcvESLHLEWQBUdAtOzOEj78RcfaoXRqEF0HR39G%2FqGgwD1wsVJdnP%2BtCpQ8YEizHeRzZ5Wk%2FpdQgk9%2FsXVC%2B6ZgSqkLxBwkIQ6VEgmJjAI8L93h3tAiNVB%2F0uEKFVn3ltd4pE89n5KsWLnb4i1pVoKpxE5Kfhvywfrlq42CKHNnBJAN1rfBQtP4YkhGJqNGa%2B6RZBxS%2FEnM67nGTExOhu%2FvcXBvpthA7eJgIyIywBlsFqJ40voWHTevugu%2FSPJzSl2logCL7a%2FfagL9kpjQeK3AiN5u3yenXGCycv7maJmEO57Wr3g5paI5K81ODjWNBmGn107TC0ibDHBjqkAQFuUpZMDTLJrDa12s5mfkjKSMHnPvq5ZDIjfPxLo2mzH%2FPM3srXU6NdQF9XHJ87wohUTIzmEUyonnNs7Mk6HNVA65YZaGbYAwcKmbOCWCequxlootFcmmFw1PD%2FsK3y84SxNDMSurjCA3UetWE72bYiZn2%2Fi4nNtRjK7XIj%2Bt%2F8HT9vjRlsk2FX%2Bubww3rPXQJv8r0DJtpCXFiNloPAppbghdK1&X-Amz-Signature=11119030de2e32ddf380013aab1a8cf1c3af9c171b8f0e829670b4a3ba96eaa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

