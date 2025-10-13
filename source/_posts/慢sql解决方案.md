---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WODLBJSX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDr%2BvHVJ6j5GFuHrmLZEJfTlqCz0ZBrXI%2F%2FhG6KMG0bQAiBYSJS5L%2BC5T3qkVnVu0bN1VMtQwVfuAENe3Phf9Xnjlyr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIMj%2FaIE9%2BB8IbWeo%2FvKtwDTlMXmRYH%2B%2BF%2BzU9HMFnzrTaKeBp7cdI4Sj5koXtRF5a1P3mNwbw2hrg%2FjantSGE%2FXC6%2FS936%2BBmG5uX79avafke1z4l0iIyeDrFa4WjPOZMKorymRQnwckCvCI%2FIBM3dexugGr9GH9s20TV9rNUjhDjAaic3viCOc0zw8yXm1Mj9k29SK0utvCSN0sEfdFcb8I87wn7T32WCp8NwmfN4Gf%2BOrj73CUbtr6abXQTHU27UFskBpo0MImjALO4FUCcp7PpZyUAVjYKPR%2FbpDNKOAwvmbNMPe%2FnzmbW4OUwOCfwXQF9D0Hpw0KdUrs%2F36sEgSHiXHerWUvK5KCeHA9UvS6vJSIR0ohFmHzlAY42gPu9elIB5XIFZJ8deuCLuzictZMO%2Btedhtqp57fOmsD9xq1Ov4IM7V6lJVBoG5M6UmrkTMYN54djsBCbzMSG%2BnncIyrzdzT5sQZFKF2aAJBBYPfE68gRYWjKKZXShhe9d51%2F4GGCwHqfJc9r5p%2BKpY2Z7vS8jptahsbyIhyNX7tBgrahJ2SGF6wKgJLi7bzqqa1S3XTMwrbDz5EDLkpElG%2BSTciq4VG8w%2BkcrDYdoW%2B9APmnjT6gBX%2FQv9qYNexSZIDbjLOk1OD5DM1vnnqowpZC1xwY6pgFs0ZIS%2FtCPAnJN6p8I28ofS8KgS%2BmbE1%2FPhFjapCdaKdvnmF0X%2BZJwfWqxefd4lp85LfeBYfGWMf2qEKmMivO4SQUFRnI7kSiFRrC%2B51w9vDDoZKh%2Fa1X4qzYapTpKH3oSH9%2BtBq0%2BazgJFYvHcrhGbvXp%2B24xv2UUBId%2FoD7G9%2Bn6eFJ1lDiDTClzd4wM0%2B%2FvuGHDcURzdETNzHB4M%2FIcmH3LnX8O&X-Amz-Signature=93db7e8c8b733efe9b93d5a88e49738e410097966091b56002f8a73a5b679669&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

