---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRTG3VC5%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIAt46lGXiAxPQelh3SFhKUgrWPio3r7adtaGEM7vaXYOAiEAzNoo97Z2gnGjzmVUHBKTqlQB9ufR9ffaw6bMsadJ4EEqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDvwFavEPxwdtIiqlyrcAx6eApopVfvPa6YuBGEbxfJ5KPX2nMFUCo2vOQQkJIpecNEBmKGNbTpE%2BPpmRJhdlB7wypDLhctwwHzir3HsO3KW3zx4K86bjzp29132t%2Ff4Lh5VNr7%2F1%2BXW8wnaLgbqSTYGdpJHxV7M5PFjNj2jipEK5LDJGbKx8RQPyytWI2OcIYRJ98%2BnZhOKmTM9BcFNRpEH2%2BKzfC09XwwzkA2UqzjNGaRiYNGxqEemWN3GB2tp0JsMubjGBkj0aZhAtHvZ4yP9SL%2BKnXrPN4snfm37SvxYnnpp%2BwNil6ZmHsu1SH8wzQ6LFnaIWKfar%2BevYBMSY9s0binpJ5HjFPouAxUY6YC1%2Bovbl489NmAItNntQapyJFyaC7A9%2FuAyFFfBone0pCjz8QuBPV6yx2ZiE1xvToLSzLgKcWIfKmn5Rw2bky4%2BlNMOsFK8NEKDwVJVnk0pO95IhXylmm0LNwG08julaprZl7W%2FZIkByjxw5n2nKhH%2FtIhwp3O4M%2BWJvW6Cpmk2xINLXYK36GXXq%2BRef7pAhIROVM4s8f4Uzzt%2BAh556HGIxzmt4wLUcQrjHs32Ax8X1HlI8ZYiK5yax8xINyyj0bsrWVQgM6s%2FOu%2BSK7H9Z5MblC8enKR2%2B%2FRYCzRiMKvip8cGOqUBy%2FIe69wcU0CPOF7xPNCyHEycu5JzI2LZHfQtHikBFlxtHqX%2BzkWPpYJT7WKNSpYuG2GLTgHW2%2BHDKF3kQGtFnTwlOZOY9JNwDtNKf%2BDUgZvlVt3cXz187IPd%2F2FvW0vLAEVGg%2BQpFdZMj7WqwbrKBm9WXsPv81xA4xl%2F%2BDGpjE7sD2sR1C1oKmfVbI9hGyrzuDviLqtBT%2FKUZiylpEUWC6Cuwk8G&X-Amz-Signature=1ab1025efea76be1c70455885b62e811630cb4b183329b89b78d748d7fb87764&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

