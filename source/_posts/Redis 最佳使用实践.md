---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHCSDPPW%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICIRp5wG9cYlJir1nG6phZDE7H1FdyxD9n8Xuxfg91trAiEA0Yp6bIUKou4wkV7Y88H3keR9g1N1nwadGb2RCkJSmysqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPjw2sQHGlG%2FX%2B1M4CrcAz8HM%2F0HSDRi9ygN7jRaHw%2FCDbiIjZP3ATNvtKupqoaS2rYxAK5%2FS5e%2FGq9T021ks9aYrtd2ZFc3XyvhyY%2FpUkqkztFUD79TYUhJ1TFxHVS9ERAuOKn9DhOr6%2BVO3z0rhPvlZnvAVEezzjCxnldxIgTi0yWTAP2b%2FhIaPnbwbAQE1CMG6Y%2FRYDa0%2B3ZsFY0y1s%2FQAz5P23GrtqiHNPfE5S%2BCEQlhH%2BEhTAAec5xovIFpyO0%2FpDYzhopki4KOM28qSUWHpDChwDG7jEK%2FsIjmfyWRGS7s%2BbNXC%2Fhhyfe0rC6n22ReMZJtDrc6rmgFk%2FXieew0WETJtM1y556g1PRlqdVZb0jKM3sOJfeyEq2PLN24Rei6iW5nZdrC4dVq7dOtsDFX78wWDhmOf%2BN64cLRiGXIf6zKS7KUo%2F9Nsc6Lhde2zPB6dsuI62gUy3DITiF7y5eytzwO4forBVVoasS6KPDqkF0TtyulDhxQH6kUJMthEwnWNAVgAnRcwqW0EQ%2BQCR3ATiKaeZRDs%2Be79aWxOATVOtchNJRu3vblTGSuXk%2Bq7ndfB75YFHwQlCXQHX4IccaQwcVRWUXMNHdPE99cyJEpsgkc44XJSxSdkP%2FGiAQD5tew6KEcbGL%2Fen6RMNXe58gGOqUBA%2FFpLeX6Cq9cxCZ2PxvyJFDpXVSCaGxCAPskS5G1qLTqBrj3Uco87pabQkhaGgLV5jV8TpQYcLNTes3s%2Bsi5p129n%2Fc02hpMnmQHhTKInUg9Iv9G%2Fg4L%2BBOzyb3hzfdr4zOY8eIAlHVWDCZkwyV0pmziDjVgoWBUHUGsnwDzKwqVSt%2BPm8IF00zejb%2FCb8Ul6zdEWhY8J2HwSFPodSHoIreZ7Ydf&X-Amz-Signature=fce1f471fd1e2fbf61451a06d3294de2dabe1d45666de2aa6560019c1e9c4b2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

