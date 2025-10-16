---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662FFICMOW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6bSwJMr93rBSr32EMDyDQ%2F%2BMbVeiGlUQgAvLIF1I8zgIgPyWZf1rzBgP5ypyPzeGRwtWSdh2ooPoYpe0oPZ9bx7cqiAQIhP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJh84FUlx5TyDbojUCrcA%2Fk8X4pMsXs7H9HJ5Wfdu7WIze6w4esOzHX8JIWUte9WZ%2F9Pv1FZF3CGNXo6KecyOwhhzdRTEp3w6K1xc8a56uYWJz8LBmgAkMBzTmE86uGDAdH7K6smku6CvDQxFVtlljJDzfwaUyFKVs9UFE4j%2F6N7NsE0ZaXssB0XMmhxDQEST2zzqtUgABJVH73ByerG92CzSe7WxYZOLltFcGLS7W9y7I4n2GUHVH4Sair59vAwo4BrzT0ahXtgjTCHu2Ur9fjZicv5ouvDRt4Sa517Un57DgO%2BEvmG%2BKW%2FJF8ZLzSXSUEyiatRczUoMLRtPx2je%2FKgvq2rXXvFyfPyPhOWo6kfZ1Dm8laSyt4s7NnaIt%2F1d8wkKHYJVn2KnjlAdkNW8kxrnsQSJ7NXsbVSBIN833J2k%2B%2BE0pOE6a0m0kU1amTwzkTAkRwYEd1nW54tgr7IRtd02xtXTuuhqjHVjGFrrrOfhkgADyfMtcPA63Le9WZI7Vx9%2BBEp4EWZd1sr3Y77zJrLUdIgYZujfPg%2Fhb1VyDB87x7W86PRAaDgeEPpEp%2BR7ExPP5rRd1Nom9TiMIkmXpUhMi2Fr3Vlmm74dCn58gPE9l952660DBLTZzXhe5UIOg%2FOCLfxTM7mKrG1MMHAwccGOqUBZ4C8XHLMAkKTC7dFoZiCgGTc9MfDzvb%2BTLsNIGlCotLZbBv%2BCAd01Dyr%2BKQEYtzNVdkhtbMcl9URsUFBUBGMrKg01MdAY09V8sFQf4O8%2F8q9NGhJhbnjOS%2FzoCOknoMF2M926Dejk4naDtqWWwYoBKoRE2VbfnXHH1jga%2BNy16sCXMYUSwi%2FZ3iwc7KiW97Q%2BVF%2F8gLfEkCV%2Fl%2FAz9MofYgLzqbX&X-Amz-Signature=9b4f73ffadaaed798415df6586e2e07158e0a3ee6aed6eea9d780556d9fffbbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

