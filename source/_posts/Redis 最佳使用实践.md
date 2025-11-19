---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JQ6V3Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCu%2FfQXvtkmZSpVYNVYYieACJiux1AGM%2BQkwMIS5ujOBwIhALPvP3I020sEUXxgy25L77PJDKsiXt9UqYI%2B3C9a0iKKKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxujie8dxHRIKYFuPYq3APIn8Qm%2FVyX4sWqSmAxu49QP%2F8bUWcWcegsaouFzGOMXLRKksof3YDB0gbegDNRJkmYlIi54Mj7hrIG09dFziQk%2B7IE%2B1VXNf01IXF1sWlfaLjnhxUpi4iqepsSwsBkNacJP0%2F81ECOicxaOp%2Fkn0EU%2B4Fed8lhkj%2FgoLMLsED3JoF6n5HEJ2mGEdjqPYRbIlZP2%2FlHAvMTAJocHXExh1gEn02TUCUt21%2FrliP7wOYWuFBqvGu4cWVa0Xdx6TKM77PlhNlbpdyVKcHK7vjzzy0Mb1%2FOXynW1XTa9NJtk%2FoTKbjTVeIhJD6LxdBNE%2FXj4uDbAvQPVij%2FCIb4W5RXfo2rQqblKUUrrGiNKYo6q%2FDQRTFNw7VYtUU7Ys79qBKdr9t9EFkFIUU0I%2B7siwOtc8bwt1H7vuSLp8TYec%2F0vNxxG5IhrW%2BnVnBZr37cgVcLNRK7e9gISJfiuaGEONZ6pfY12UclzJVIPTOX%2FFVfQRdLhuDL4ENrpimFSW2oFsD8vlHR3N480zA5dMtfvquRy6tXqmCvIBzWZ974io7YcI6Raq2Ojzq5X1zp86wj4ONNDtV2A6se7UU0PCWmbnlHKRQtRgNWw6%2Bso%2F4vfN6U8uAnJpv%2B1u4hkmRnQ3FURDCJuvjIBjqkAY%2B9fY%2Fca4k0LPlCNOCzAMY3qkQgT91xOpzG5xkcnysitMCpcj3%2BbmwTDplc4yi06AqUoqtD4Rea2j36ljPGffOt6RnZr46Mn%2BUZzkSscIq3igQ%2BZeSisvovgxHY8m0tLqgC%2BaiuCxUhh6kPThJtIlwkH210QuGPfyKg8V%2FWILZo4bYChr8US4YknCxTfX1%2Be5kOYRtdJ2vJE4n1jR8Qv5MzlrGj&X-Amz-Signature=b5292e6d1f67382f148a2866ffd85ee26adbea816044bce29bd0211b83c22aa9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

