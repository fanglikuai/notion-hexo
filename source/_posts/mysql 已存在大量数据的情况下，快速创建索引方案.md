---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBVCDDME%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJIMEYCIQDSw09zqoA8kDeeDIyvcrD%2F0EHdq%2B4d7%2BM5b2vUfOXlDwIhAP0k9qGSSs7dvirQVW32uWblBBNbm4db81nmWHi4cwj9KogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw8eZ8ngDxd7ejSbs8q3APrPfi%2FPAtLwOcGzT7VRvbMKfrXE3PEnS8d7Kl%2BkEAaZLU1Y2Q1FCHV%2BmVbncy24IXDpkPytaGt1dl1hd6tnH1s94YC%2F64D0ZZFz65Zxgji8xo%2BybMmw6CLSd58YDCTvfODcYuNi%2BihegeTYwPtk0srLeTUwBUjsxoC4XGPObf9xA9p5zO26IxvukEIrThOi4fDTCrhmPF18EXNbY3fOTjjqHK8cF3mnVA50yg7z9kwrfsqbuGhgNCB2LflXX%2BbfcaHZ%2F0gzLeIbzkxNS6vJRTbh1Ce5hw%2Bcjfyr4HdXwcj%2FCsGATn3Y8iYUtqwBJPAWJ8mEJ4Y1ei6yIQBjluy7wrKG6%2B0GRbR380f%2Fe%2BRglOSHf9lt1d2hkAzG4z%2BgSdtpIC4167QBzF3nncH8WiRCc6qiPjbHTPmXy6yzeVMX0og2%2Bi60w9Idhbd0vA%2FIWS019qQmDyZxN7WmvhYz6QOt8Aa35RLv1Svsb1%2FjzDhGd7yHBt6ZHyncO%2FDPmYQCS4KyNUvq%2FmHWb3EdFzhNRaGLNGXACCVPEJtSfCwSgoTNM2KQXehywmHVbd6MG3fNqsxLpc4vMr%2FNPnkeNRtq99zaRgfekUR7iogOsKmQSMXMAlcLXBazylVw5h6il8AFDC81qHHBjqkAfZS8n4lVPamC7SwWWWDtYFjIxI9R5q1fIWuIvcaTJ0RgI2C1cqyBigoV8tRwURmf6I5SmH0dRBmFbI0grogjwcfLmaJbCtr%2BrA1niLiudHPBCrsl3lwxtIEtJvCeCeDPy%2B%2FFX2Gn80i%2BeBYGaSRO9QWTJLMJ4%2B1C12grcGFFCTuuSlZzOCrG9LReaVRndNW2eBmcEqXd2BAENSOcvLZ7v3pfvmM&X-Amz-Signature=65c0dabf14b753054629b114de87951866ff2061cf47f16ecf40fb85350efa66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

