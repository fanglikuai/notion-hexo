---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Q6CLRFI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCtYaJgiBCt8FWRBADUtXflWjduV8loQjKX56%2BLbothIQIhAOP8OGwknFwAikz8IN54EFdFjNF8Mb505e8BtJrFmUvTKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXo7TuTYtBySvBgf4q3AOD2YBS2drZe5TN%2BphrwlNGMA9F4dcv01zJbB%2FVItxguW9FIvKa5%2FwAYZegjjVNn5N9791W3z60ry0l5olYei2dWyo0U089ScCRHPulE0sxgjC6P1CPj5PkZ5yHo9y%2BRmGAb835SeTox4PvRjpQcIEAPwnaTHOPOFm%2B%2B2UFQCZr5WjNHGEji6P%2F8N%2B%2Fc4SZQktFUOVJsf6GUGmZ133A8WzWpK17lCI1yKFKUGuK35VCp1NPmsTpbFNQZRURchQeWueoXPall1dPgkk%2FIn%2BiDVOQs6y158Fq4DibDtPhPUfLdp76rVL6KE1D8zovfK5L5EaQUD6ZMKMiS8%2BjFW7utkAGkHnG%2FtVv1dm2j82vCg5GuOw9KmYH0bB3NFn6WvX9zZBmklTYkyMiXTwg04AAW76tgpCu0jDKYlcZ1VzXqcBZnJQyjiE8d9f6al8pdCu2mMlC88tx9YsyNFL%2F2LBeOyuCqa%2FOnvN7QC1%2B8zgtWQElr7SXGwaOY5VpP%2FQchw%2BiN0dISwdZP%2FFI2pOhqe6Z1%2F7NNro912BAuzBnfIrhl4okdsp03VC14PuzHlU6TpH%2FBMmJQHufZE95o1qmFSksfmxROEK7s6JNxYtUywS8Oav4jR5p9sPfZ9jj3QUC7jCRquHGBjqkAR6ca2%2BSeY%2BKXIlUMFmpFD6HSbuU%2FXip64iMyA%2BZDQoE9KVeIrCB73GDBJvvzoZ5YUyIWVsGUvAlfGc28Lld4UdZe%2F4Bk0NU0ef3oC9Ad8KOwez9NCEOCjBW3A%2F0n67ffa6yNuG%2BnOilQGAo7AeEyot8QgsfnQWdYvTEGTSy%2FW8nXCU4I%2FUXtGMDX%2Fgei96tPO42dXzbnI1Ugj3pTwz%2BNEAeJyeB&X-Amz-Signature=6cfeb5d578b35b1ef920a413e55256cf0b1a863f29ee5e6d437fd918e0c31892&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

