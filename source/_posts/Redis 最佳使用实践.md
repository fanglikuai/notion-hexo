---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EKL3THB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T210115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDE%2BhBhlEyt8GEvijMxuaLp0RAUeUXTZlA8BkDkZVgLzAiAVJ3VzfRSUTehd9oSkh7LVoFWrcz8azHpOiL2j%2BHkMdCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMO5WzsoDSkdRfJlGOKtwDxWXNimbqjxNmu8hrJbVi6xoAGLfWhj%2BuxIDwGGNVCxWrt5kg9QuowXhf%2FcXuDRyZUe2O2Xa6tU6kAikGXSSw5fvDv7PEE6%2Brpd7Dnh20KUpfovk5yjcSJGaLJ08Pxsk2kouXz1SudyPq4YQ0Q0Fr7FQ0y55HzjtmpSAYF67DkXZG1xlyNz0ArIGJ%2Fqwlw2%2B311vF1bzZwysjuuA3SPvddCgSyOQQsOGdYPPWGcBmb%2FaGKutu6SfZDivGj2ORalZtrOSvjterLYG%2FUkS9C8VZuxGLsw3QdjXXZe22Ue01WwKj9yR%2F21NTG%2F1Keam1NSOYiDQBAaNYjQ92m20RHB2kzNdX5cT2LMR7qs%2FX9JdtSip6OLMAmerEQd%2B2Elf0HlBteP0YzDAnmicfxpr3Td4Jzow2791niW4itniXb4VeIz3%2Bu5jatGi%2BodbN5uNEB76VReNBLRlUR6HJR6kv8bun9%2BkDQPfgrcBnpOkgEH6ruYe3trSEeUxwWjMPTe0hHCClSj3q12lI5Wyd7tsTOIFKpt2wW%2Bxj49BSrUsEJROxnQIrN22JTRAgp%2FH%2BP6snMHsvcMjsXDdanLkLJXyzqiCvVBmiHIbACP4Cx55Vaway8b6WeqIPsAU4rbuitXQwzOiKxwY6pgHmne1EJkJoS9hexm%2BOTU1%2BTeQNLCg1CrROyD9XbmwhvKpMUVDGt1QjkhwL%2BFaCwAEOGNuNKzwXRXSrjFzZ5ZLOZfP963PmH3n2yFM3Ohv4AR%2BsNCIwAU0KhkHjiOomB6T2Wg4cjJ3jUmTlBffLGZChNOVNj3UneAwJNACu%2FW9gSliaI%2F191iNlaAGZFU9ilQ8k4SvIxi6pj0U%2FDLkKY01zNm1xHR0%2F&X-Amz-Signature=5b566ea0e92d66bd381c22029642e664e879940efaaaa2a184a1608716e50882&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

