---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J7LWPTM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBpCXT%2F1QXpQG6vq4OkmTsXDHLBDwTpo9kXvMxX97ThQAiBjFHADEqbx1vstGhBhum1ix4ziUevh%2Fzfl1dEIzWsTaCqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMc7LB83oQEr6UWQd5KtwD9Lwh7K%2FcNGKhoqn557I1%2ByaNAr4dV8578On0NTR4dq0pNrOMH1ZIFKRNVDaV42EwNb05xaxEAldJAZfmdN%2BxA%2Fgvm7ncZNDfxbzQavDW4z3%2F2Xa4MPaz9UMlTltflxcRELVDRAEcCewoabzHIA0yuZSZ1ePgZMmjUGv%2Fr9G6ZIkWRKfCPXdBlGgTc4ISlCJLGt1EcnYcAJYZ7rRLwl1CFkNdpgofYA8lbPkql2BMwQvNlRcLaob9gP1s6rHuDO9ySf4Ug9XsfdwsvQUkWWP1irhgDBjqzu3BomfhjLIAgsrk3Orz26HD5SrFToS1f3kAK4OGWxo%2F0PzCvCD2uQkYsJRSOJSvxy7Gf%2Bd8fR8zZ%2FfbNE5paBUfS5o0BiSY8ouKT6wlrsVogY2IhuRkyU2BeYGyjG9FsyDSHTCTZMn2mU%2FGAvhPmccOjDW98kHvlJzn%2BuCljHa7gxgjipdtO11unuQfuWaSeXxcUekGo2v4xVinU3EoVPtedtYz51VFqyXgVMfwiopGWMB0MzTO9grPwsAWtHQV3tV6ShF%2FLt29AY9eWlK3MkkvtOt2oCV1zAngSS%2F4ySsBUC%2BJLAGiJVNuD3%2FraPolYDgKagEGyi9TWXOC5y01OKHO5sx%2BD5gwi6uQxwY6pgG11t9QbaoTnPpDEBQbIPpZb4JZ8FnbcgnV8X0TzBtH7KAVDtEtJVnZdD2OWHYY%2FcyGVfVN3jOYvqYslmOXdkqed8EPgdo3ZER9%2FoFKlKmUFL2b8rEsNsNivpA8MgZnVHDaarTwsKPZ1FNqbD%2BeN43XogjoDeaYthGrAgc8%2FB%2FowQP%2Bp22JnMGiJHPUh9Mlp4%2FvYi3oVIjYNV8k4rjgeBdvel4tFfO%2B&X-Amz-Signature=71cc51a8171aa3121081c5b4cce0245ee4f8779dada1d2b05c524934fb6d7313&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

