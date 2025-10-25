---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QKTU2PN%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T020050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC6LOv%2FZpPgDLuEFK%2BX0GCF6snRe0%2BK8eH4Dm0sYg4ywwIhAKr4%2BvtrLcGtWXwb4JksTCT0SE9cbb6jcr5SJuTXAB0aKv8DCGsQABoMNjM3NDIzMTgzODA1IgwJVe0BfdNcp1Xxpvcq3AOZhYWxX%2BDp%2FgOXZTbBSsX0WVPYvuduSQdEhGLJOuAwzHAfT4Xkxv%2BI%2FT%2BzZWA%2FmdWoonlwUPFAaWN9vETSlJvWbtKfkZvmiQczt13ulXIl%2F5IoKd%2FREd8moB7ULtNPBvp2d%2F74NRBZp77TQLnrsTd1cXvNQ5ZvOfFnEXu6N%2BXF75bDjwa895yzD7YdevlRZRkFR5kjpyooY%2Ffgd4LrzQ%2BM3QAJj76A7TV32d4siKJhCeCxAlHV16ysnrbZ%2BEYeL82dxO1c%2FdcOO3n%2FS66UxMg22oDx2VLmzUod7FPfPjDotnWG2ITaxHHuIsxLy8mn7BiP%2FdX1Ub4p%2FzZfnbLF56A1JBt%2FEZMJEE19Q7GLFAL1%2FSeLAEXh6%2FxbDoKAng64zc4B%2BANtrf%2F4G9NtMRfd5pLAyLZ0i5HSpfCU1USS6EYgrqTx7Ho9XkjX1aHCiF84GpWC1DIKBHPVA0k18xsSp4xHn9ldMJjiFUx80wkEgzoHqhPtruroPZUEZiQ7zYAULejt25sP9fNbxhe9KYM9amHB6CbilMNRGKbgYSGWYTyTatFlUaLDysm1HCI2QVyWiUS6F6ATwzhA6onPL%2BiUOmR7WlYRttOGaQacORUXJyhG%2BnOyYn7kYtJiYhmUMTDU0vDHBjqkAfmku5Vklt%2FVagXw6ptKLDX0ZWzsY5sGQyV3n1bnISKcJB%2BWChpO4HIFhAn9NT0dajpRoVt%2FIuJEBnBwQHUB8OyWGrVJEa8aB0gYxYvmPnynvtwn9nQaoYoZidUfjjGAyutPP0ozaSm9vjLoLrcyXmzeqEgQO%2BmJho%2BuFIKXnKTC8Kma9DY4ebe3Ub2mAN5bXjt%2FTFz%2FwIHC7V7hDga0SSHb0L%2Fp&X-Amz-Signature=afb800cff770ea02d9d62dfd2a7587cf9b305bd658aed44addba49c0cd749190&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

