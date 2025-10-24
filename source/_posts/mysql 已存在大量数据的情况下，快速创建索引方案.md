---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UYHHO2%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6zB79FmM7bUoYLQ1UGVbK0kr7mPhMjnJXGaiVlwqV2AiBn1FV%2BmVDKEN0HCbOOUcNGrspC79qy4%2F8SAi1lsPCJoSr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMc2kYs2EBz3xC%2FmPxKtwDVsX8tRw9QoAt7KaOhQNw0Z87SFqmGODRIQ8L4DyvkwPwkQNzlQclx8Kq5NWhrZua12d0KycwXGK6hFfNrPnGd4gqRKFHoGJZUIU6rkFp4PXnsEOBqx4LqmAWwlOULDxFqCzcyrg%2FVy9rr5dz5KAUlq5z%2BXgXNQQDL590qOIjyVGNbAA7AE4ZGXK8crEXQzhOXBquodac5PHaSmnchOOXHFyBPXaRlKy2ZUevc5ReDPKuW8Q%2F5Obq7bY7UESygFTZ4VTKSTrMH9yl0gGD%2BWnN2wL4F2Z0uydn9kRATCdOXOx9FfFXu6ukcA6LN96I2cs3rUtxgUvXFmAvQ%2F2aHKSLUo1n7n5PJjJ04Hf8zhJHxyRYnnRVPMep3tzFvy0KlfDRJSRnSb%2F1cwUgLu7DCu5jZ7B9m%2B%2Bsp0KTWk5mIt9uZaTeDomwX2k5RkiFYA3EuOL52oawTjU4l0938WAyTE81oSVRzh%2BN6%2Bzer%2BZ6tPq5XMfKxewM68A7qaNjcD%2FI%2FYs3yXHT2MwsGG%2B9I2O5agTfcAuVW5H8GX44fuoCn5HEs5apd9lBapKrGoEoMIIqRB6qKsUi3MwJwqbu9fGx%2FqeBIasz5MTmkZPihvyY0GcYRWdIrB9U2GWcKLGliugwxIruxwY6pgGK1f5F1fDIxtSxFE25fzGfgJQakoEAhGVV4GulpbRjP0YPuqhzlV7SNpjFLwjmbxwjjbwauZ9kGqu77LiyjP9yq5bXt6ndEmvh38NVMINirHpijcWCb5jc%2FRYua8NBefh06krARue4RB6QPIyroMb80VpI%2Fs%2Bq4iAPg0VvTA7TRSoPGvA8j2%2B3XqDytVtVIFlpKvaIfnXQtbDGZLDO%2BW0wguFxYPUc&X-Amz-Signature=100fb820ac57ac85c06a6fa2b0d430efb8ed1ee38002eab8bd068053c9bfdb6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

