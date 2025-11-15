---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMQMC4FN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGwaMa1yJ0wa3ay9faNJYsIBtmXTb3xm5dq54tkgKtyHAiEAn85FF1JNY9h%2BSqCN%2Fa3vfbbGKsfRUdvpgAXaY%2B%2B5SZQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDOiJmE4kwCSI37UyoSrcA5vYPnjThvsFDcciRNqKB7DMkPRo6d231cDXm71I5ut2QKAO%2Bp7NYk%2BsT1nRV0rjeseqj6k%2F%2FJHg%2BjdoSxRsrdJh3I%2B9gGMsourXafVF9WhWUZ8Ln9HzZT1ypeihaerI%2FT%2FIpY77a8g%2B64AghPYTuqnS8XBs8bwbnbOp8yLqtd0g1ECm%2B4Kpc6AsCJdQdJ1A6Hn0gHP9344OWxk5QVX%2FwCtKmLFCuNG9%2FRQV41JjOZ3t3cDsiHlSvk7dYTr6NOjDW64%2BLnMOoymXipLZ8GG1PHTY%2FXhE1SQsRlB5gAmgjPwj7YldIEw3D0iYvQLhKw%2FuP1ib0QzCu3EKlajds5h0otssoK%2FUTi2GbFjKhSuwaSEDF7O1%2FQZgHrZHTbdUqxTiaeeHot7xzlQttsk0SxtbPrssujhsEqxjtkuQTlGdi%2FcpoBYlsF0TvgVE4M7B9XCwQ54q78BohbOMpWdBLfAm89TkZ6nqJWEVYhleTHCWR6BVuP5PGzlsFcGLnbXfRiAVoQWuSMV2j72ZMWyPGQk4DvgE%2F9Vv5Et4NvowMyvmFHgzq%2FPgWl54NQhvaxNtt4zzJo%2Fto8SWdb0F3ZW%2FsH5aCysRyjwxnlLD5mVQPeXs%2B5QHEvqmJD%2BgWI3Me%2BZuMMDo38gGOqUBJZviKTaPjIwex6J2O4iIvXzVt3troGX9ihevoMsk2OuhugF2gzhOX2IKWo5aQQ7kc4wkIjP9o00eLVdV%2FTgWaXBgjQ8N61OR9Di62%2BTABVlrwzzcwzodzNMvZzE2lcKZztRZF4tc%2Fv5Z%2FvdPXc1fWTiWInrl6tYFbdmUxY1DxW64A538qvq%2FzgXMZ%2FpfvB43O6jWdJBg3sW8iK4yTyAtX33P5PbB&X-Amz-Signature=b9894d7a424f2ffe9e60835f4773161c384e92c21cd0c7c3fbe1ac8da62e928f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

