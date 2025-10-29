---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QTKHNHR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJIMEYCIQCGl6CAQl2cpQtCti9H5maTFvFE0mN8dd4khHFkNk1I2QIhAL1F8%2Fog6CJxKYMAXVvjn2UyONy4xc19HzsFvF%2BGIdBqKogECNr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igylj7jNfsByed9KgHEq3APXRa5FlsVsEd2e2ttyYVLfdcMXLd19e66F5pXV2sG5eMk7dFf6wH39sbkk3IwT25mo9uwyeZklGuBsYl4LY82Vj%2BwVc%2BlmDcD1aynlEoiUakAYWcgM%2BobHEEBw9caVqEfVo5iy%2BaVC3ujIH7VG34TOVj6fRGgtUL8Kfnhy5hVn%2Bov7iTN%2FgSiHCd9msS472HAZdXp87ZfiGZMNeTEmz73IW0CwEc%2FRcFvYPPg5uZvW890Fqv2JDMNfcjACL%2FtYnR%2FjxoE1oP5UAyjo2j%2BlI2BWJwl9k%2FiQroxbcmwzZVH36FNcXPriYJvZ%2Bb0rlh9ZiLQjAO5gdXLqfxgfCqCtTKuCMbcRM4uq5VI0BBKgQvat01uQRR84xaikW%2B%2BNd1xdqVTh%2BgdoW3O7jLkmRmUyuxMxvaauKZFZpWgU6drhy4KL%2Fb06h1XvGXw2glo87j0wOp9cDq4NxliYt9%2BovuE8Iq1sQL2Eu5fFcI2dDjDxGvJ2zrSK8hVUUf1pAB8rK7xOBfCdQeyxJdmhKZK55s9LMD7%2BKeUenVKijOtYJBbWxY65T8Rt53MFA5sgTa2iN%2F%2B%2B8x2c32Ki1mI8QzEB%2Bt%2BRmCMkvaALXr%2Boyd48YCJqS%2FREziHjZeTl2i6Z1YdIuDCrnInIBjqkAQbVCLCv93lTa3v3e59kCmFQrNWl5hfZUJ%2B615qWJH7eZE%2BLqm8I2MBYQmr3Ht5%2FCTMS2P5D%2B%2FxSUyGbn9BVAU7lX2nmzIJ5ELS%2F4r07rlAPMhskMR0EI9hXvx%2F1XICndwndoYepvXx2URhlj4GbjMTAgCwxvuXlrDHJmyQCim3qvGk1tPV1AWpGNp565GQAA5EgBn1pfn%2F5VtF6BQoAy9FIvKuh&X-Amz-Signature=8eb86c3acff828d32940fbe71a9c1ddabab6398a0c3d63fd2c99b8293a71f047&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

