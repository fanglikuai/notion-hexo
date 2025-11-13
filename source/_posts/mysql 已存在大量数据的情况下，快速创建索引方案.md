---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VI2HNJB%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQC4TI0uKMo1JwoGI%2F738x1kw52BihDnR8jGWyotzAXmwAIhAMgn8WuniM7AlBBI5zOLa05ailwtwpzd72QKqSlZpVK4Kv8DCEMQABoMNjM3NDIzMTgzODA1Igyu8aIEm1XrKLUCu64q3AM9KdOrtp5AzfXMsh3XdYPKnB6fuR5HFp93QNoWkBJe3ffdln3tb4HV0IYpz5e8l%2FZD5mIzJG4kWp9ADYoXqp6XM5hjS8LRP3zBZOUL9V6C5DV%2FvvxnShLzitgT%2B3WzQgJBJ%2BP67oXuWn%2BOD9Z%2Bwl2fX9iispTpB7Z%2FygI0l%2FhnwxNzhW6VXlGrxRFpkYyFSWoPKm20EHJTj1EKpyIyCw8dNR5CpScDTU3tskiBmBkHIItIgloxoSOTF3vrsqXQh1NYOw82HqCT9TQvqq5UaXAeA4D12%2F7s33fWA%2BtOc0vK9F9fkHOxJUvXS%2FGPwlk9PoyPe8cr5egF1Yf8URfXpOD%2FsHAz2L6UHt7UXHiPo%2FWpdCBVcGtGgNYsmGDjMvJk4J%2BArAiyHK4r3hqsjS3BPA8zl3%2FZXwmFL0iewFdfAZnDH%2BbQtAI365kzmcm3dVuXO6R12a%2FNEmiRC%2BvbjsuvAFjrV1mrHVQqnZvWR%2BpqRlCz94vNwof%2BndBsNQNDK7sz9ku8tLKzLoJMXKMSCebnxa0goTCMqw77hVYxnnEKgJTd8%2Fg%2BkjkJg10Z2gghEnajYeFPmGxOae2kp6bxsGraI1P1EnaSIUxdv2RUgZPkinvnLwepPuUcpesMAR422zCJ%2FNTIBjqkAXYPy1cpvN964iznqCJiJtOjzNs4l6d5UhJjuVtNm4Pj2ki3PmcWxrERSIY9JEombeSp1Yv3bp9CSx6Vm79SkGtBK0IhLMxRPNZCJA4WUtdGYZkH6y1FdtvuX3GwlSF9k3kNPjWnz7OkpmCOTUt%2BEsX7teMbbz40Ve%2BX72475DivB92UeZ%2B2dKHPDp0g5KFykAWgM49ALkMyRqAbynyjDf1loLnz&X-Amz-Signature=7764ad49782cf5fbe75c1fa280a20296c95918678c8e107d2bdcb3f3594e43eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

