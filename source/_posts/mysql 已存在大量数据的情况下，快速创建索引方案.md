---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664R3OLGC5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDunFpt9FNrovL3hhHCV7yzCs3oxDa6RJ7FEqOdlo6b5AiA%2BuPMPiZfkrbySIoNWUW8ySMfMFY%2BCKQ9tFNi0Eu90Wir%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMaxakw%2Bk9M2KtYPsgKtwDDRjOqqFJU3zOJIO7f17F2Kl4Q6DfOByCsikl4dQQp9L5Xb%2B0zmjOaOD7co0X2k0wNMRx6EbIYFI1Pct%2BZ5F8tFqbVCHrIf0UBEb%2BNyVCOoWgakrOujGkJJSsGlYsuV%2BYfKb3RAiJ8Z%2BwjpeKrihyCeQKEBRlP9j2%2BkrUNdxT1yKr2w3oVXLekOVx74gpc4UvgTbWjn5xomWGLjTUtT%2F9VgXB30IJWgzc3rce1aTpkoxbE0dPsMNDda0GE4joAF3kF7CLAXL1%2FxD4JWBnsd%2B8Ij0n2quMT7DwVGoObiqkU9DfZ46%2BWYhpakrf7HWsRpSw8sWsS0uEVihck%2BYU3JTVY1QGjNmxBPgCFNJVoq1d2DsmnmS4XprURuTxr2eOHgK2fVyQHCJHyzDPRwmDpL34LMcZ6QiHMY4QImOwuR%2B3v5X2UeEmMd8ucToxKP42cvK9fCHb4SjuffqYXjW0YNcZLjOUGG%2BHW4PLNvU5KmU12xl8oLJZoU8za2Ocj9feJb%2BDYRvczKAn1SyVHZ48fjfVfOPXz15BUjKzRO5o32Ikjudd6agXS2l5oXmbnrZmF%2BWiOrhruxPqjY%2BhdG6gMxK6KSHp%2B2grmh5uxaZG%2BC%2BQhLqD2aPDh2Qbky4ZrpkwteGDxwY6pgGGj63paDhFQkm5sD9ej01gTvA4JQzDqhUoq6m64RU9vgb8tMfjS%2F%2BF53ke69yOt%2FetlgOawQOXe6jEyPWlg266KtpmhT%2FIdF7YE16X7kYcOww0trP9GDd8nupuFNwxBczKQtQVJ4D2oW0MsBHwasdsn0ucTSSnOnY5HpdW%2FJGOrs%2BnGWzaUL9Fg4iOXQZsRZh1AzLNkv6GYJ6AIDWJyTqbe%2F6uMU9M&X-Amz-Signature=9df41ae38e95adf96c5a0e6a362fcf64893f29a2e34995002baedfbda837a576&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

