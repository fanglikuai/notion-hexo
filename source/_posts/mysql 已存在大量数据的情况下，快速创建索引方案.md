---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GYVDTO7%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJPX2nlIfpx5pyb28ydm7y7ek00M3ldaQuKqdmuaLAfgIgAng%2BCPs5HDpATGdPGUn2hPpXKdsN3sJ8fPOVUweWKwsq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDMddsRw1FJFIqPwtzyrcA2x1ep%2BxxjaNQr04Gr9KtxaRJOHn2MNTlhBsleP7i0NDsLBK0GsI%2F59aAPLhV4oHd4e7DFc1DRqpVp%2BYouP6V5bDMjEcPEZzi6jjXBf8zj%2F7XMGwsV4i7azMIUKu6vuz1embjrh%2BXOhEjqOkV2Vz%2BvtWodsqGI7D%2B3OWk9%2BHQE3%2FcFmnJqH86pnUEMgM0pNK6BSEpJAN4nmaOm6XpTHlskrTXOcskatYcrSi3MSjZgYO95HmsOR7wWHkLJabS7a%2FUTAb2WDQSVUINS%2BGGA1h%2FfFilDSoi73FRmu1whxg5MR8LHqWYZRYUdvBTMb1tS0tkCdEa7xzoM9N8qGVKu3IIgK6EKnPmkLQ%2FuIHbzxwod1zSyQGSwJvqZaNK3%2FegJ5yk1fNBr8NETv5kqZmtu2a47Z5nH9Ng1d3OvPBO4KgfO0caNBBLTBL1UVbjqlGYJQBo6nvVHwaMaTIm%2B%2Fl7geTuZddfUY%2BLx%2BQfPRjPopT%2Fo7AOSk%2F%2F9TiwG1qXyW5NXTmbGMP%2Bw2N1Fs798qU%2FdtgNyG0DWfEZWObh0VjnZ%2FiLe6w29gElCMMGcAYiSRTmgrKmPIhRfZ%2Fzfwk9bkLI9X4zSqgQfL9BIGwI4tmBqX6K6XVoGy%2F0Vg0WaiTKTiIMOPzx8YGOqUBo0b%2F8xG0kMspoVtti03cC0mVlNVvEVrH9H%2B1heYuctVqUCo0NdnoieV18Wb3OyJQrCA8LkSf3ZB%2FdBHb7tgKAYX8Z7QzkiV9o1%2B20TaLIXDI%2FCb%2FUHdztMD1Dl88TODer4glkwR%2BBEAGbLbWYzlVjFKn02wfGIO255Ig8CHIqQIKkmA8LmaOWRdH4GyZ7sbkQ1xd9QZ3xw2LkYVcqX3Ad%2FB7Lmdx&X-Amz-Signature=40fe8d47bc3d3aa8f9792aa97e5e127591c766088cd41434060662c978e5bc31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

