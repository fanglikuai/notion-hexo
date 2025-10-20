---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FKD4M4V%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJGMEQCIHlowWt8vQTeAvLkS9VNGi6h9VEt5fdNb%2B%2B16D3pnYNaAiA3f%2BJ9tNt5PM4OBkFJCgB6sZAPojClH1uEy16d97haRSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkqcvuC3KQZ%2BIeK5%2BKtwDMjKIL3v81wftvfI6Il4QrtKUDWkrvcrm64nTtCghvo4RDbSsFcTHo53J%2FsUu4jfU0%2BxSVfex1N8ddkHMuiH7CFiVdsZXk4k94LUB9%2BLpGTVKq30dRJhVaKzm7OvTlxJKiWN6Ii6foft%2F7a5GQTOgkH7gh90ni43fWqqQF1dcfnmKLdPR%2BF4vuuBKxN0CFtRsr5xP4r17XMjDq0mSNwdOv3XGnXycv%2Fe4qEtCg15yJnfotrvyXxVsxoC5nyjQidP344sz1nndXEp1qkS1SVwwMFy3ZD7OCenVNlVGrJf9S2K6a6tj4oMr0U9E4UlpMfV8y7rkAq5r9cxF67zw3skTysQUQys8EloyAOn24BUuIvmnbkSr6UUH3DTMowK1pVsj9VBB386i1NGfjk3mRDarbQ3B%2Br3jiswXy24rc6NZYctRR84HrT6bv3W0JtLkq8tmeUFECOJTJacqqoVUxcSvFhhXOqRU59SDKOef84ojBcH8RcpiT3FsdArHi%2BtzRVyfrzkGJjg%2B6bSXqMDnU9l77EO3Lh9hgloYYVnatiEgKs99NMB7zE0VaPAiUHCFNVdvLfMrtEcVqyUNntcj2y571R5ynXHY77MjGl44B0puXCEoBoTM%2BuN0vcoDfrQwg9nYxwY6pgHncS16y1gC6USYUzBUWOFBaur5%2FwS4ypP53qmbbzWEK7BG%2BAwphBPzDO0iZJLEsoDDBzz0tRybdqdR8M%2BSNnFCTkijwCu81NDNhUMK1bKHDJQSIFdXMo7EL50ZRCJ4RMIf7I2H9urLHcs3WxwLt0N9Doz7BIDTlUU3ebeYlDOpILpwXDzTobU92ETKJSVIa6ZSNTH%2FAiKWq931TlYq91X3ezeMkZkw&X-Amz-Signature=7af8d224ae4c11e7c9b1581f7f0bdb1df8ccc20b92e69d4a474aed11e2c71294&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

