---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWUANZ4W%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFUnJW8Zzy5AMDYTCNLefc016%2B1zWctGzyHFWacsj0%2FAIhANq9V7YOZpIkvHEkX5%2BSsPcMA0URbpgLBd%2FYOLJvYKoYKv8DCC0QABoMNjM3NDIzMTgzODA1IgyesaIB6cFJdGOpQYwq3AOoj5dIeuEwzGb0FGPHcKsz%2BdGMTbJXYO7G0kYHL%2B%2Bqbbqlw%2BgYlxKL1buqrz3tSBAha1a3RVfOrldPmvN3wVUmPJMrSttChj7Tz546X5js2IUOpbYJNVaGhNiA88X2CvyTpsnn9S1tjlBtMlUgk6k0r9s24oZ6%2BxOYwfOe%2BahFOwTRSLIZUGQerjalVjxr3z8eLMryYaKKEHhkfxADKyDS4tNBhwl0Bp4GpXlNs6I5ogH8AtQwvnMztP3GO3m2uWYH9HTUxSWxRKzfVNqwm691ezVE4e%2FhN4ZFqELvZ2A2I25ox5HsPZg24c8b25T%2Bloz%2BSFgdPvEYeMQEVtQ57tKa2oa6wcrKlwCW%2BG1i68CacIUv9dNpLfo%2B4H%2Fa5pvJGiHOWuL7WOzIXOZOiBN3itUNNzM66SRt9ijxjXaJBFqt8Lw6dr5jKTOSpHrQUTW1YcitJhbmsV8YCiYy%2BluSTkTq1PTh1B2CN9CsdTD0iXdkKy%2B511CPMB8%2B5s5Ilevg33jnTDW3pV%2BDVuwDjkik54ANFdGcxhLe2pk5Z9iIdpirLzwNweQJdKAMCaFCroB1D8i2YxiTNshCljb6bgflP00oPi0cg18GxJ%2B1chEJzZGbQdK04kw9J7QL4WRiMjD1w%2FnGBjqkAVryLpi8mcV7%2FpG0swXeQ0935WyJGZ21cSOazmJruhTxu4oF3WNWBDOvcila6xx4RYnzp2VrqehfdvgpuHc92EQ6aLN7JRg4IA1r5G4MZ0vsXU46b2X1IGCddwaAlSTrUbdsAjgtRExDrQ%2FCaQdlv1NVXY3qbPK8H4eqLQ1hqRH8PQA3kL8qLfPBayAW0vUG0bEglz2szRo%2B3Hlx%2FVh38ZB35yxK&X-Amz-Signature=fc461613bf176eb1e15ea9e3e05eb1c6c80aacb0c51e436bcdc87c91f596ae99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

