---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EYMOJJW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIBABmpghBO8x2abjNlrfL5XauG7w6TENFmedTbQ6ZemrAiAPwAO3eh99qe%2BtcHKPZ7M7qNN%2FfXPReq%2FxKQ7M9uLQ8SqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWuwfMrzqFGg8SgaNKtwDAAm68rJnU1uUjHm1vBF6KOJ8nmudcq9gbWyb3VakjjqhHj5jnfPqeYz1PNOkhQr7MDBJvtpH1JGgoZiOXp2IlF%2BLmOm7WOKG1u7iB1%2FyREK3wu9SVZ6imw2QOMRk%2Bxh6ehUrbvD1C7JjVl6BrJhOHBMQnRQMVAJmfl%2BqfBTJkz1qb3iWARRV8SIzVH5GdHZerCr47PH5e0ao2OnoLk2SLoPmidATCqeywOZd79fIjALgXpUFmSN7lBYULQj20wXsmfa%2BDVWvcBWypBNgDCJ12elbUD839wQeoC3iDBSmvvDp57XzZvK0M7M%2F5tTnG%2FUx81C0Q7xJHW76pfjO67aVqBGxBf6i9XTPy%2Bme3UVohFYMaamFkR%2BIbOv%2Fb7QELVRgfhAZUDMegT77YlWJHIr%2FQFRnKN%2BZUGQGi1rYGE%2Fns0a10Z6fDX%2Fo22O2mdmWyCJvt7afc8jGDcIfYldhCWITfCdpw0xEUHQRrAyBxehb91TxMOZIyuIOZiwqXNPMN%2BhZwikp10rKEDxrCi4OiZiDRMcHGLULRToa%2B6Ju%2FGh%2F%2FKXnaKCpL0bJlg9WfmLNxA3hwPne%2FEjKXGncNZMUbKjdp9etntsUMUm6JmUhhB2Qz%2FtSzyoNe5fxdsbKLUQwp6icxwY6pgHCK0EIihSj2pbd3umq%2Bt%2BcyiFBdiY17urQPIrByXavZqIRemcX5%2BvvrTPoebsPqvwC3v%2Bw7kateaEO5OSWa%2BWKzx%2BT5JRK8gGv90T0v3mEZ5Xu5aSYEU1GGm1tYBaUeFZLOtnZDVC7gK8zO%2FV2CiueOPfjLpOpv%2BO%2BArSUvg6UTInVz01109X2pWy8wDHjSG0NNNvmktrC4Q4IUGUBPEPepfbTgx1Y&X-Amz-Signature=0ca8fb8839f8a8ddc962b13c686b30d714e24314b53f68193a589a7aaed2e4cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

