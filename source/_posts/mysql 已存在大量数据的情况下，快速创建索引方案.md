---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624DJ54R3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T190200Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEPOFg3eJf%2F%2FRoSjGmKzd1vIzOoJ8qh67P0OoRNV8%2B1%2BAiAGVcafvtPOR8EmIM5P6SWczSlzwkxQz9MSgqCZdgj%2FvSqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMudBv5rTKxTidZ9GKtwDh3XgNCTseuWWSDzkEJvN9RNzO7cMBi6dDM82tsR9Y48u%2FJw8qWC7lB88pxG7f7L4xBFcCx%2F5Jo4xDC8Z6eiVjKZJv3Yclb%2BI1%2BPhFUUB5IKYzIjODUl7wt9wnp2nM%2FJbIV9ad8AAbxtHD5fu2kKMMRgyE4JMdVm59P4ubJGOC98ntty8JWN7MmCOmdfBfS3CruP%2Ba%2FW3%2BzBNDaC8ysRkEgQ%2FatT5S3QRlqrU2Se5aEhacX9zN3Ixxj2RGboKqeaVSgW9D1E655QPTa6MgyCYCgW7OaudPULadfDeA81Zt1dkU7FHQ9l%2FlJeMH9sA4DrujvO3xmfrN%2Fpeu2Fu57hEGiQ%2BSk5lZdhoxhMterdx8bx%2FtOsr42BN06mtN3L%2BYvvpp5e25x0heglU1nj5FZ3q4XSvvcNHX7uajGsNXvCqOJD1s9zMTMQrrGDePIwfdJdqzHU0zRt9Ywdq8Uugcri27SvoHMUESYs1Mt5AdMRnO163ysganE4ka2uL1OXooUYQ%2FPWIIFUBNIbd7%2BS0rqWiZZmrHkfvaY5XkI0hfa1OEHr9tUXvtj63FxCQxYolG8KkiZtLv9nCNK2B4aWsDL%2Ftg2PQECa4r7WiZOni3GnrDRtAIe%2BHBr%2BbMw2giMgwhPfExwY6pgFWUhd7sEuIpAbPLqrkM4%2BXg7iv60cCq0tBJaGXa0GTEewockQbSC1ux8WDH8CkYhgJI39D5dldJS7lKmwNv%2BRWCJmrefS8gpCG24auYXQFwffSoHo9YvsGTgxBVeGaB7idaGyTMcunnx1Vi7gec4zEjgGTtSLlXJn2wZiFLzhcUBNOMWy6UhONNfB6r7maRDqo9VsO59wN2XiHkJNC8X%2FuN2nc4LmJ&X-Amz-Signature=1793d8a524f32e1e3c2978128e5148b84c46d5cb24e6f3a590e3f746a1b85199&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

