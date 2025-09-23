---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GYVDTO7%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJPX2nlIfpx5pyb28ydm7y7ek00M3ldaQuKqdmuaLAfgIgAng%2BCPs5HDpATGdPGUn2hPpXKdsN3sJ8fPOVUweWKwsq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDMddsRw1FJFIqPwtzyrcA2x1ep%2BxxjaNQr04Gr9KtxaRJOHn2MNTlhBsleP7i0NDsLBK0GsI%2F59aAPLhV4oHd4e7DFc1DRqpVp%2BYouP6V5bDMjEcPEZzi6jjXBf8zj%2F7XMGwsV4i7azMIUKu6vuz1embjrh%2BXOhEjqOkV2Vz%2BvtWodsqGI7D%2B3OWk9%2BHQE3%2FcFmnJqH86pnUEMgM0pNK6BSEpJAN4nmaOm6XpTHlskrTXOcskatYcrSi3MSjZgYO95HmsOR7wWHkLJabS7a%2FUTAb2WDQSVUINS%2BGGA1h%2FfFilDSoi73FRmu1whxg5MR8LHqWYZRYUdvBTMb1tS0tkCdEa7xzoM9N8qGVKu3IIgK6EKnPmkLQ%2FuIHbzxwod1zSyQGSwJvqZaNK3%2FegJ5yk1fNBr8NETv5kqZmtu2a47Z5nH9Ng1d3OvPBO4KgfO0caNBBLTBL1UVbjqlGYJQBo6nvVHwaMaTIm%2B%2Fl7geTuZddfUY%2BLx%2BQfPRjPopT%2Fo7AOSk%2F%2F9TiwG1qXyW5NXTmbGMP%2Bw2N1Fs798qU%2FdtgNyG0DWfEZWObh0VjnZ%2FiLe6w29gElCMMGcAYiSRTmgrKmPIhRfZ%2Fzfwk9bkLI9X4zSqgQfL9BIGwI4tmBqX6K6XVoGy%2F0Vg0WaiTKTiIMOPzx8YGOqUBo0b%2F8xG0kMspoVtti03cC0mVlNVvEVrH9H%2B1heYuctVqUCo0NdnoieV18Wb3OyJQrCA8LkSf3ZB%2FdBHb7tgKAYX8Z7QzkiV9o1%2B20TaLIXDI%2FCb%2FUHdztMD1Dl88TODer4glkwR%2BBEAGbLbWYzlVjFKn02wfGIO255Ig8CHIqQIKkmA8LmaOWRdH4GyZ7sbkQ1xd9QZ3xw2LkYVcqX3Ad%2FB7Lmdx&X-Amz-Signature=e07f1f64e3e64afabee04fa41adb9058d31132c1a3c1ca7bcfc2d1eb57b69832&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

