---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663CZZGHI%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJHMEUCICrfZ8zPY8ykuJETs6WPqNp69KGYXSkzaZpc%2FfD%2BJGlDAiEAtDrIDwt%2FyxW1HV79put9q25TZhC6PRa8oAT4rteWPwsqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD0XqFWxfaS8CzMemSrcA4%2F42vq2JDaoASWOniC38RylKtO52lCojVgNECg8IasMpLnzQeoVCA6FCLAxJ4wy6D9Dj3zuc5zF2jgM%2FxZ1ZwVDz1rnFne6UOZ0z%2BhVpBUcXvcfuehLs2%2FAuCFuCCzr5ZlCoRAXZ3FOb3Pq9WM6K0pFlpLDongHV327ycXqNNJIhbJcuQ78S6aINWjDHDpl2pfyndvxn6s1UV4KOjXQECFBLs2CWZmpbNiVLPmRPBt5QMdCkdDAFaoqG6t4KyZm824lBgiQApVgYrNKL81gUzg2ua%2FK59nM92BO4jJ26pTlIBz9CHg25tKagfRY%2BoV5ByjHw4XD36Ik4zlpZQ%2BYJ4MALFAnRidxCqqMn%2BPbeObnseAS%2FD27t9wq4DwbGcPGQRrspg7JqBV4J75de%2F3QDBLLM4JkaVdGrAGc1pCoFZ%2B%2BhSTw%2F6kmdiuOnSBXdAptSgrYD1dZGKENBVYR3KqNi0tqmKOYwgukoYIGOJJhqrcyaXrs1iWeny6cznApDSDKSP9IVW4X41MAe95HGtCGQ1gWX5UlsUp6oiQgraA20b93ZCn1UO6vz90ZnD6u4zTnIedYAZrAtLIFp%2Bio5XexiAAA4D2raQv%2FJdOR6pC1jrM9DfjFhQAAUqQTUDG1MN7N8sYGOqUB0wC2L779oOzAwe%2Fh49Js9lCtyqg4th3smpMZ7rboVEXlC14FzwhYUkivYA2NSjjBYi7Itm%2BoNhP%2FRSWvBi5w95dxRqznq1aZUI5ko%2BD4t7DxF%2F5%2BGCDcuvhwLDKXtiUOeZ%2BlPD8unPmb8k2bnSsDnJPFmfIoWjsIwuE1KrE8pEaHxb39oOP8iZQwsAyWRZ9xoUOnV%2BRF5AolwyEc1yA63GB7IXuV&X-Amz-Signature=2bdfb2fbd8bde9e4b46fe346766a61e5b6bdf08c163258b4c64470693906d91e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

