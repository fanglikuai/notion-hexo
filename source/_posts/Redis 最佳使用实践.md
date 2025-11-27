---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDOUUZTH%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCB8jEHz%2BVKmrYd%2FeYPbS7zKF%2BZc%2BqrqtszoegyB7p4VAIgC%2FRaymRR%2BzGk5J2%2FxgDgujvdxiLV6ixtz%2FxJfJNBrjMqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLFeOlWwtx50h7lzircAxJx7sRgCWM2YyxulzXR16YBCg6HyXNaRrFy865aQbvwv99ps%2FWAwrXZR0GvV6ASwA42zQh58g8WNkLp%2FdzwI%2FBICHzLV801UJeUR1S%2FEqmenkvZxYlu%2FOmRf86aIwfkOl4Lf%2FZHC%2FwzZT1DB5TPpCD6pBdMydDO8%2FjhMyC5w%2FC3imeHQINnRiy7SJ5esF6hsvfHmggfiCSTKWeSv4HX14KB1p5HdWkWo0HbDpDe2a%2F1VRDm4Jnr8Sex68DLX8g2o%2FF4gFSTVSzy8O0z8O42gPBo1%2FdUd5y5lhGIGE%2Fo7yI8I2gPfcZYqlsRWqTa9cOoAgSRQ82yCYPhkT6wF7VddKAiQo38KPRcGrVlW%2BjIdGJdOyrdjGp3Z4IV4lto1C3yJP7lnKyaqo5S4F%2FZCX1TahndC0Ia4OKJThUcNqI%2BaP%2FDNOA67lHjGX6EQiv0BkbpeY3amrBAkekl5qclX4kACe48KASMnc5OTLVzufngdn3Y%2Bdk7exVeWMlUla6X2Azhqj3mQ%2BHpFGAf8ji9SQLi7aKhR6bj4L14Gvg2ki2Q%2FysQchWaMpycUGFaVWO6LzrAn1B4pfS%2FVNBEA4AH93C4VNsF4rdXZuLi4yKehz1ZohGk0Wn%2BgxCaIjrz%2BRcAMOGioMkGOqUBqJt1zm8s7ll6qaORfG3LegVtIiLEy19P6boR6Jp65B00Y09KcY0YIz0BzWaC9OW3FyhSkh06imSyDhDprEx2sPiFRelb4%2FtlSUaoDvv2E2DY2OTM4uTgyureqd5vtU%2FTUCc%2FaNM8SdJeD%2F%2FCeY5yAemBgHbb74lFR%2B6mQIY4JW5MgPFh2ydZtAt%2FWmJv9NUC%2FuXyosfBzOG%2BuHQ66wC7Lpdkpfnx&X-Amz-Signature=38e4da7ced2da8f5ea9d3415e32aed7508ed43767705e9b9cd2523d7f614d5cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

