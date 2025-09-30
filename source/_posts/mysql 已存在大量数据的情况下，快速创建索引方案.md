---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RHBEVX4%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQD%2FUxjuXU%2BA7SkEcct8n%2Fqnd0cPIZ1doOglF0%2FuThRlQgIgEtEg8N16Gv68CvWQYGlS1V8PUStghYclWgoZqG7QyrYqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA1zyadwdPKM3F7PnSrcA9pZkyu4X799CIWV1tMh4jxWnTG%2F02xqgUQwkQHtPXpv5vgzBl%2BvApUhxEcPhQmNLlFpcc%2B3OA%2FxuhrXeQdCa%2B78DYtMDQ9FlGMxBEEzQzfmdQGx60KNkayroyp%2FfTCiTQrk%2BCuCsAQjMJ%2BTEXB%2BNjroobAenJ5sskXOs6pGfXgPRVmG9%2FBjMNGuRbsBCGcp2vul57xXTlxjDtkHRc3QHDG%2Blcwnl9neV7glkbCbUwPK4eZFEd3vY31jaPDK4b64If5iHuSjZYmD1zOtZFDx9cs4rUOy7YNdcMviqneH1pTyxbNbGfhul%2FKF9WKy%2B5GUdNSZRnKTp8pOPlHNLDAd6drclFqiXEFJsSaf7Gt7Y6X%2B5YXfOxUB%2BgPoqQsby3CZWZ9H9EgnMPqpyztSYVWz4O9qGfCrcuLIYVILpEhf0dL5GJ5DUk848NO3zndYmk4JZvbqmARklOCwrT%2BXDnoXLxndsmo3z2TGYn%2FmcdEh%2BtnJ1a3FvemgkaNCmnCkk04l%2Fz2d%2F3N6MzLfPC%2F0Ji7PgH3%2FMIc082SKiMlq4pcILAdjhamkFy3zJmxJ%2BLeogc9HybCF5qzlrq9NWr0tg%2BgLcs%2BNJH2XrZnvyIrce40HJlRgrsGBMLXfXUZT8apfMMfE7cYGOqUBQSokbUFuQHXFfIkO4pbXDQYKSqJJonU0XJu0jXni4GxyCF6qvX1CJd3gaGa1xq4%2B%2BQCjpxlyiI%2FKwq2MH%2BQUrsMfPaXWfIfw0tGVJ4wJl01Dct0LsbokNhfkZYv1tnwslLB6FClfxxh807ttIuune2m%2FMC1YCgp2yQw7AZB7q16SKXhyT1vpIdOlRQXIO0p0zczB9uC65MFj%2FBS02OHmS5IMrsF2&X-Amz-Signature=90e6d4aab987705dff07f9f0c9a6826c6fc4b2ae654b7481ff2471dbb39bcc91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

