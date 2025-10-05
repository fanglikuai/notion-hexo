---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKWK6XTG%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGmBgTn2bKCEBUjoSkb0tDPGN4vu6kWVoD0HueSWd5DBAiB2P6BWhGuzy7nj6Y9mkQJ6YcCVOu5LghJ3z7xk2%2FYfair%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMcbYhqDKDbTa%2BV%2FOVKtwDYkLVu6snXY6az73egpYrSfbMha4lzA1Y05%2FE1x0El5JjAJ9Uj8KFwbOWkQV9C39wJoGPvYvE36%2BsxyOeIMF7C05%2BEIUKt6GcQcvoubq8%2F2RTFaAHSNQ5VGC9v96zzGfoNJ7DLbBiEV0C7z4R5snjamHRczNox0qroO%2Bu2ZRWu%2Fg%2FKwVTHSCGy0gs3Uvx5mHNvwakqFVVvorocGJ%2F6LqFtwkjJi1o5Dz%2FKLJGrkjh%2Bubq%2FGFgWgbpYaLdf99lbZl9s71tuiUMUXUWziyvxO1uTDVdPTvy0dLdHDjsyE1Oq1iTajz5AWtfrM%2FLrr3GKrxJyE612mCizZJdrNXlRIX9edhCZ619Ru6g5WWcTvfZCRjdtyfc8Os9NFGfKKz4uLSm6GpJ%2BuwmqEvmMRIuXwIXbmBtXg6SdtZBSfoKRxlKP869nSbjU2xvLtkEQBLXc%2BDg1r5VeblFfKAdPf8LkxlSVbPXgmU7Kd3qDKVPypiiWmNY7WuMyonS2ZFbtcaJ8P9CifbSDHR6w8Tt4eUNU%2BApZznh%2F8rkHV0dgZcMmvKMjA6Xr1ZTdPM4A8RQvOdmmCU%2FnU7c3JSgEJ%2FzDUg%2FubqFBlmtyTSEEwyxnN8Bf6ygo3o21S%2Fb%2BfxvWOuVA4owheiKxwY6pgEe77%2FjLOJdkb9%2FMPYbEOIC7mnLW1XIGf0qEgJXRBgsGxVi6s0FcPRgLVfIajeiBtFQTIyAmjNrBXCjMsHYQdOMRU4db6pVvtquf13jH%2BdsGRS%2B9eoBzOevZpzwG3MoAw%2FeETgjyDW9C0aW%2BbCaXQ5w%2F0YCRSazIWt2o3TYqNzk1T3bej9HMWjA6cfTvwy4lujF7WSA0tBUw%2BtxmQ4o2VdypEZTBCGj&X-Amz-Signature=a719451531572f249b00e6531e829149520f4955c99ba01c4ff7f9f822dd07ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

