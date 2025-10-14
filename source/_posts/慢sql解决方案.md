---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNBJX7FA%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD6uwOtUAWYwa5yULVpaWIT8O5m%2FjTuntcxN59wTMV0mAIgOBWhFT4basiqh%2Bs5YXl2qnIgHGuUIqeTVGpA744y%2BqQq%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDHHl1MjHHxXawefq1ircA%2Bn5Kj3g6CbQgF1DPrsFrnsFcqT9sVoVtqsXhDQaYbZHvB7ySYJEUHzjxs7YNN6eiSMzd2wDZ0%2FTKm3DwSftX4pQXI69ytgPo%2BsXRRhDSqMAlSuvpCSWIpJ1EB1hdQvWUpH%2FjxoTrb24XbFxDuPyR0uKtvlFQ5gxHox81KEyOwYSplMJjjPpCsWUb2vg9Vv1Hty3Y0V69NhrRoOJbI8aPqfyKRPrQcwDwT3c7lOu9s3XCxLuWCKF1GVWk5RjUwfDp0DoDO52wXj60PDm4eZqGF9CvIRDGxIPWG5uhhewJCKHZ5dwMihD2oxTPs%2B380Bx9jXUK1ii8aBTYxWkmIxjGC8I4sotgRSqVqprfn3Pfox8%2BBk4h1StfvoYmnLM%2FoDbM6F9Vm2k1qC5tw%2FIAZB0ifY9JosCXJQl1hrqvgtiC09l3OlIvQ0HHXNIajnyeYKtVdouMxwmIBSaUKebRZXYyh87OnG9eUcC6lUWfANyFbH%2BmaZA2F53rVi8zWnjtGl4DocH9Ov5o8pTR5SgOqjZnARo2WB4B0RTSzHuqUfThGTw%2F1NVxQD4OH7n1u8oKi%2FNzCGOkRselRQ223Ybol4wi85DG%2Fcesvuf%2BKZaBCb9dcJ3QCxI%2Fvk6kHyptqmwMM24uccGOqUBFOaS%2FV9PCXD1zRJytDYcc9lt9qk%2F3%2FwyN%2FFrCeHR7x69eGgvc0ItvwVJRcG6uORifkA37x%2BpRXyytfPdVx4tX%2FF0xGD1li8%2FQeizvJYkhWR9Xyhk9X0jPNy93BVSiqT6T7iYkajxRFmPmfK2Bd%2B%2Fyb4dnNnWvbGlgr7CbHuKh%2BwMmFJEPU3d4aqF7AqMsu05m%2B92OtRi24lQVnAzhojLtWLAPSCz&X-Amz-Signature=8689c552ba00be0ab87173557615584eb93af9318be04b5b5dfa5b1ddb0d05d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

