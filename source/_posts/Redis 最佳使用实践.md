---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJT7OWTO%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCZQEEWsUlnDIRSni%2FRGGcioqyEh%2F4snDnoM5I3H50wQQIgLJ7WWoD9s8mNdRFxnGEo%2F6iV%2BC6yxmWiRFdMv6X5H98qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI%2FMAqET%2Bj3gRJ9RVCrcAzxcHqMrXjvHXFi1tNbh1UMRBa8346zaEH%2ByGPe0isobJHELvcLobD3Psoo1MjIEXKgSwdYfcr9FtsJGTcZPzYMB87ZO288HJ62ER8O5LLisdjVDjD2mEO2nq8SdSLenHEy1PS1Fyna5DnBYDFfOm%2FeKClXsUobtkKtJO%2FPNccr27JuQa43fgIZ19Wl8xxlB54dm3bAl0GNf2RJYuVH3djclCTqRIRLf9zvjPAIir%2FWUMucDKGRWz6zZ%2FkAXNRcufRtPYNoygXUTHRgumHDvvF3JTd6hYLNeHDCjayRa5H%2FkCKneu0Vl%2FzulrHUKDCCUpMeqI0o0hVbq%2B1QsGrFa%2FyIkZkWnFn0Xh%2BJ5EikfpKoLxVEhm90Fo3jDliya3BxrDKDirxLqW85DrXoCKlMuqtyv7xmzmJAhzNz9Az1zwXkjUtYgl842jwtTXzB28EQtglSM1derW7Z74wEWQgeR8WNweU87hDhmZL8t%2FhWS5n9wJRsPi5SklOkAvHViEuiRmgnp3tFkYmI7%2BnCoM%2F0jL%2Fnz9py7%2FkhnKlsXdVbq19DZjurA36I8WHk%2FIZ0Gez4Mz%2BVi234d29kqzqxdVIlQsxUTjNDl4h0gqTauZR%2FgaEJAveAFUenHgzCLM%2B%2BvMMm5s8YGOqUBkBkIgNecbWJmWTTStw7vTTdTkP%2FU5fECv4u7Aj%2FKX1yyvhcNreb1Rqo%2Fv1WwqnfIsuBp13gk2rlt74K0M8Js97Dnf0gHHLESF%2F4rZdebLCbp6h8FCK9W9BFfLS0EEjPgm%2FM9TTdnqHEmvToDEU7sMHVJUHcycF8h68PEZYGSbtBqyN4I6BG5MuZG6R4yLeDwaxV5J9shxb6kGlKZZM3i%2BPs1uL%2Bj&X-Amz-Signature=63b29c6b33d8f78db1fd7cd2f3d8408d94fd53486f258d9d3fa84b6e63371cdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

