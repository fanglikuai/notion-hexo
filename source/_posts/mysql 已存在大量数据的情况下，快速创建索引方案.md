---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNBJX7FA%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD6uwOtUAWYwa5yULVpaWIT8O5m%2FjTuntcxN59wTMV0mAIgOBWhFT4basiqh%2Bs5YXl2qnIgHGuUIqeTVGpA744y%2BqQq%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDHHl1MjHHxXawefq1ircA%2Bn5Kj3g6CbQgF1DPrsFrnsFcqT9sVoVtqsXhDQaYbZHvB7ySYJEUHzjxs7YNN6eiSMzd2wDZ0%2FTKm3DwSftX4pQXI69ytgPo%2BsXRRhDSqMAlSuvpCSWIpJ1EB1hdQvWUpH%2FjxoTrb24XbFxDuPyR0uKtvlFQ5gxHox81KEyOwYSplMJjjPpCsWUb2vg9Vv1Hty3Y0V69NhrRoOJbI8aPqfyKRPrQcwDwT3c7lOu9s3XCxLuWCKF1GVWk5RjUwfDp0DoDO52wXj60PDm4eZqGF9CvIRDGxIPWG5uhhewJCKHZ5dwMihD2oxTPs%2B380Bx9jXUK1ii8aBTYxWkmIxjGC8I4sotgRSqVqprfn3Pfox8%2BBk4h1StfvoYmnLM%2FoDbM6F9Vm2k1qC5tw%2FIAZB0ifY9JosCXJQl1hrqvgtiC09l3OlIvQ0HHXNIajnyeYKtVdouMxwmIBSaUKebRZXYyh87OnG9eUcC6lUWfANyFbH%2BmaZA2F53rVi8zWnjtGl4DocH9Ov5o8pTR5SgOqjZnARo2WB4B0RTSzHuqUfThGTw%2F1NVxQD4OH7n1u8oKi%2FNzCGOkRselRQ223Ybol4wi85DG%2Fcesvuf%2BKZaBCb9dcJ3QCxI%2Fvk6kHyptqmwMM24uccGOqUBFOaS%2FV9PCXD1zRJytDYcc9lt9qk%2F3%2FwyN%2FFrCeHR7x69eGgvc0ItvwVJRcG6uORifkA37x%2BpRXyytfPdVx4tX%2FF0xGD1li8%2FQeizvJYkhWR9Xyhk9X0jPNy93BVSiqT6T7iYkajxRFmPmfK2Bd%2B%2Fyb4dnNnWvbGlgr7CbHuKh%2BwMmFJEPU3d4aqF7AqMsu05m%2B92OtRi24lQVnAzhojLtWLAPSCz&X-Amz-Signature=a3502bc0817b6e6e6f6c4b704df5686f20da4685bcf7e10c4050cb15fb45c9a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

