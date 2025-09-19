---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46665J7UW7D%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIBQ9j5AlF0Dh2gRVaH5CpPTJZs%2FNZT%2FoLWt7sIPTmJWiAiEA2dtZSKr%2FHAsBC1wthKyTRbuCXCIw09vTAzjyIFx2MGcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKy3%2Bi4OrL%2FTxcPdNyrcA5%2F5vo%2BHjW3fJz8h%2BzXDL2fNfGK3pcCUTRAkYcgf1Mk73Z%2FS96s7mxoj26bPQ501V94YN5Z7fY0g%2Fc%2B8njLy35Ckz9SigpYRg%2FTMuF%2F3b8ZPc6XRgDr77J9uYnpVEUZ%2FhEYsyj4gKGwt1iE7lm4umkDpywrMSjxvU9USph4EqolWeresDCSokRmJlwmV7CYzO50PIQrs%2Fd78%2B0%2FlK7hWmg2uuaFvAKKu4RxLtuZVfzA9HiWVPm0Wqz894nZmoRjCdzzWP4bmhMHCzPTZcYmsctIVJrEAqcFTAOd8dYHOLXjOaM684CKBR0Z4uNrybsj%2Bm1X7%2F4QMP2kmpIHviZ%2BVjOl5AGsICQcAEIxBkrwM5rV5iWg1mWCWa12MpLmxNGsTgC3jDVMvMCMDNusRlk0k0KelnvdB9BD9g3%2FIwML%2F0KntIVEj%2FElJyl0TSt0d7V9D6k4OQbeifZeapi18V3JKVqZMvnadRKfX21Zt124dgwhOh2re0smcIAyzLcqGtt2KZa7%2FjVvEB2nTgPuV%2FA%2FwIDYINNmRlDUiV9DR6UJGjaS7jdJFgOPp2ZludjOtD41xFSFwbq0P%2F08bAYgf9TAqlxdUsf7aNKdLo%2BEi0WblDASS2aPs4TXZYtIrbtI7MJmHt8YGOqUBTOt2lEx1HAlJ%2F7zKYfUKWFqC6wgbELY9KTpyAMi74ehWZbu2sZkzFjS2gRuCk5lvxh53xc0FdqSiwqDEsSXz9rM7BgMqKNbaRiezkH3s%2F6yj4DuhgXNxadUmfrH90z1t5L7dGyI%2B850ErSuizp%2FYORY0lEA6RXTopYY5oCUXvBPEeNQOuq9A%2FLaS2qo3FFJHsPHFiPrH3qe2n8Z0bNK3o2H4XDGj&X-Amz-Signature=ed0d877954960b4a729259713805f1226dd6ef0ecc8a88f03225f69104c2c395&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

