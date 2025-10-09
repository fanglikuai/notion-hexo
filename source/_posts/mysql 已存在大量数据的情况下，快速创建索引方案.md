---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675XC55U6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T030114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIAYppzUCUPlwa9idFGdoi9oUUryUmh0bC8SMA801oOVEAiBtsg1dhbiyXBXL%2BExIRR5cvSqtZAYpQGy1jgLk5lYWGyqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjtWg%2Ba4oFBSXky6iKtwDIF%2F4aB3S8rzHiSNFaYmE86ruB4YfzerYaRGR1Bl6AM%2BsYPfgR4iBQphljWMxiA0HJm5a%2FxAZRUUjr4MqPuFdqWor1giR2QMwbUudKvKea4QoIIDR9q2QbWBucxexm9yUJ8E%2F%2B2EIrOYgwqqj44hyphHAPCILAH6GcaQgk3D2xX1QuRg305kD6PwVFVkGkr82G6uaDG%2FJ%2BUyX0L8YxdV05eXb8Vkcy%2BX0bMSJQoRqL1qvr13YR40BhEK1Omwa%2Fj1rH0dolmnu%2F3XcibMQxsvj5twmoj8iOVqmXxxGcHmnpYwho9ppU2OXrZ52Cs5W3h4FhfpyDGBMCPMYOwonJxernVj8NTK8FaYYot9VLN8V8d6nK5g23aHDci2omxhC1PlfQ7X8aUZn%2BFFYqsBLtbYz6C0VcEqvJFO18qX9IvquPboP7IsDpeduTEoh12p1CIxShDOCbO7HhW5iUsUXvZTWs3I7WvAbAhscJO6DOZTbiO%2BaXYhdnyFCx2GoKUnt4vLQxfzuS9xwMFaeE1RP935g0MYH9mG%2FPCKwVeEuGjKpy9zAQM2umuRJy2KKHONjhGcfxNyWDkwVbY3Q7W2CjCckphK4wXn7sggUAGacVeOgeTg9E3%2F6oKUIt0rigLUwhqicxwY6pgGCrXh0T1qgXOsCqxirQ8Bq8AjB9CGylormeiprZ%2B5CeiDVfKq7%2Bo393GXow5V7jM5dXkXMWSBb7PoHbwq5uj8aFz9CSZucJT3DvvU3JiKbVw%2FoJ34sCUbffwmIcgZqDUKWTU3cC5I2whoLZ6Ti0meMXFfuYjLeOYUrc7tDT7xvKtTDjgIiuajG5kwKaNah7Yr3U%2Bmoyv1C5azIhmUmR2%2Bso2p3i1%2BQ&X-Amz-Signature=7a32db3d065acbfbec3fd1113f1a997290f1ec6b5052fb27ae786aa8e2a4c658&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

