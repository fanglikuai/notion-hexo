---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XK7BD64H%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5gPTORWipV7vOqa4k9%2BofArT7jMM3IEkR6woyxKcXhwIhAO9kAIuL2eWAawFjnnaCJTFZoq%2Fjjih9gOyjovbesnBnKv8DCFAQABoMNjM3NDIzMTgzODA1Igw93r7MGCpqm9yR5lcq3AOxpIv37UUGhrirkvSbG9zmLstyifO2Th7AsoRDKzO86OsX3PIZvU8AM4x9F1MHaNYEZY3spb26x6Lk7UyX%2BXituCpTz2LRtvKG2AIl1Hohf07v1evg1tfW2DLlIyz4DeSLNP24wZeG6ovJdIqjqd66gmV06CIW1r1nMzZQUsEYMJNzcGc%2B898MgRHnghyeA3B09Vhiq9WUpVewLGnCKe6YJ0JLy0suY%2B466jNWEEk5hS3NvtpElz4SzOx19K5U9lfO9xkd6NxutsjLeVSBXxJcVRgfF7AHisq0N%2FUL1wxpPMimRp0tgysVR395VOSdK12hNDE4r3s0TDNgaIxa4nJkNP4T%2ByEpJ9PI3FMRTPWhKMqPsYuAyHnkSH%2FKDD5y%2FXi01RnEuYtezaMDGHwepuPrGKWVmCGUJRnkelX2N6CnQqOKOS7UCeHj194uVTqDEv6ztpc%2BP4lNqNBQXAWCEnxCpoXWIi3G%2Ft%2BYSJstuw25orzu0UEpFbfQea%2FE3eysBHKtn4zqp1OaGrKtvVP577hes7CnhGLSLwyUBaZY%2FHoAkgjkJc%2FEsjxtGGFTNdDUmUM%2BB7UDgmWPoDsS0fu1qHkIJoHQ8NKLebNaid9DZMlilvc7SFQq%2B0TePEOYPDDxhJDJBjqkAYBM2AJRweHUnnjPP3dBGvsvEGAw6o0z758lxNGUmoSUSrh0jYSlpJXz00uasId5hqdq1GYDHjy3wVhjUqqtsmsujFTIbD5auMBi6kuF49RzfPgdm4D3FACvKscfk3LrW48%2B07Dao4AscnnXSztXRtBiQdkIq3tBwjbluDH3w3UgeQch%2BQuiXuSz3wq3mQl8SVWh8d702urJHF55YBlpSamiDWvp&X-Amz-Signature=64abcd45d69623a217488564bb3fd3de34b24215ebe240c71fbd5aff31a686b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

