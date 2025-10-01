---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3R6K5FT%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCICK%2FeuFlPXS5r2wAaUb1Vq%2BDTGM0Jt%2B1%2FMKHlA2xe2h%2FAiEAmE%2F666PTEA7JFyrq8W94b2MNMtdGkxbt5mfw4227Io0qiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDImjtVKN0qgI%2FSuDkyrcA4XvqSumICawkTXwf%2F%2BcWCVGdYwdhbGo2DJTb%2F%2BJGjH0V%2BoXe8r4G7IccnQWOCeiNRsLL7wksW8wxXMA1LY9q6kcqyqvyxndF14Frd4iHneUk9o6n1lF9nNMV29s%2BhOHzjYS9b1ZXvl6SNuKZ73nS1bV%2FbpRM%2BVkHlOsznngHwm8KTezw5n5LgLMIimKuA8nHifDxTh1QSQc3QPfzpTEHilljuz5ac947H%2F22zwnImID%2BYnOeyj4TQI8uHQUgBOYQwlRcsQizPwkf%2Fm78aTJzTki2c0QYkOBc7ucY42JVMI%2BxIy0yQRAVaHU6ez8sld00gFXwSFKnc07Ew5Ms4dLQIFqA1ppCRfTxw4XuCa8FgEWnZHSm%2F8AtDNki3a%2F0fQ4o%2B9Bc4trzzocglstIgj6Kr555dZuToTnJMzCKf3GBvnRxDR%2F0qiK6khFtZFn7RyK85d79UydOe4U5UwB1LU9GImvCLjyqNMgsm5mtUu20%2FpzW2eRY9j9%2F93yYax5F0kvjkmp0D8lKAlC9ySWS8bEqRD8YSAGAqRM9Co0ZR4ehgywbOUX%2FUmse%2F0n%2BIoUJRJUSbg7e9sszbq7KpNsstjYnkLZRlC%2BHzFUWr7ayNgXCH%2BummAfzx2sMKP%2F5UbDMMHq8cYGOqUBrVvxRCz9DVxAa0T7f4eO5J7nYe%2BEHt1icM9snI0Sa4xsQoz9Ichx%2FkhQ55wdlAkKegkNnKuMefkU1uwZj89ECNUB%2F7dc%2BygP%2BWbNqm%2FE%2B3rlUJaTB4MMZ3lSz2IUhwR2YNoKebFkZpoGeMRVFiPJrCxBNBVSB7AR%2FT0U2iM08b%2BjC8Ng71f2psUbRTnDFeRiyBkzdKinNatCQoOm0B8K3h8%2FTxxK&X-Amz-Signature=418e23c1ae025cbab70d34c0ff1b9e4fd5cdc28cb71a95beed01776bc684f841&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

