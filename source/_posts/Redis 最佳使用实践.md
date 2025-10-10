---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBVCDDME%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJIMEYCIQDSw09zqoA8kDeeDIyvcrD%2F0EHdq%2B4d7%2BM5b2vUfOXlDwIhAP0k9qGSSs7dvirQVW32uWblBBNbm4db81nmWHi4cwj9KogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw8eZ8ngDxd7ejSbs8q3APrPfi%2FPAtLwOcGzT7VRvbMKfrXE3PEnS8d7Kl%2BkEAaZLU1Y2Q1FCHV%2BmVbncy24IXDpkPytaGt1dl1hd6tnH1s94YC%2F64D0ZZFz65Zxgji8xo%2BybMmw6CLSd58YDCTvfODcYuNi%2BihegeTYwPtk0srLeTUwBUjsxoC4XGPObf9xA9p5zO26IxvukEIrThOi4fDTCrhmPF18EXNbY3fOTjjqHK8cF3mnVA50yg7z9kwrfsqbuGhgNCB2LflXX%2BbfcaHZ%2F0gzLeIbzkxNS6vJRTbh1Ce5hw%2Bcjfyr4HdXwcj%2FCsGATn3Y8iYUtqwBJPAWJ8mEJ4Y1ei6yIQBjluy7wrKG6%2B0GRbR380f%2Fe%2BRglOSHf9lt1d2hkAzG4z%2BgSdtpIC4167QBzF3nncH8WiRCc6qiPjbHTPmXy6yzeVMX0og2%2Bi60w9Idhbd0vA%2FIWS019qQmDyZxN7WmvhYz6QOt8Aa35RLv1Svsb1%2FjzDhGd7yHBt6ZHyncO%2FDPmYQCS4KyNUvq%2FmHWb3EdFzhNRaGLNGXACCVPEJtSfCwSgoTNM2KQXehywmHVbd6MG3fNqsxLpc4vMr%2FNPnkeNRtq99zaRgfekUR7iogOsKmQSMXMAlcLXBazylVw5h6il8AFDC81qHHBjqkAfZS8n4lVPamC7SwWWWDtYFjIxI9R5q1fIWuIvcaTJ0RgI2C1cqyBigoV8tRwURmf6I5SmH0dRBmFbI0grogjwcfLmaJbCtr%2BrA1niLiudHPBCrsl3lwxtIEtJvCeCeDPy%2B%2FFX2Gn80i%2BeBYGaSRO9QWTJLMJ4%2B1C12grcGFFCTuuSlZzOCrG9LReaVRndNW2eBmcEqXd2BAENSOcvLZ7v3pfvmM&X-Amz-Signature=23559edaed724f3685670db5e77e2271b59022de25e9b85a192e7b96c02dd3d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

