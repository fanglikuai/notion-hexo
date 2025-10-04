---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653WPTHCA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAoiRfPmElrUXaIb5YWQEenxG4e2Ss2anpt6SrvbrZujAiBOaInzAqVtCJEbdMEcJcLCevh6xGYPxgCKhit4MdAhpCr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIMoiSiu6a0FeF4RTKLKtwDj0RQyO1LCw%2FPMX6m93qPVu51AByG1Ml3CWph3ft5XFGB2vTmie7w7jaqQXVQqCddgXQ%2FYl%2FHSdxFRiahugosXgOSmZawyghZmGK0Bh10fGnZw6TNqPDhcZZxUMZMmGIbAv8MPgiD70UCFFJn%2F5hoC%2BIUiwnZLITrvqr1gd8NCVA09S1TF5Mz%2Fey9kThEEFCv34GSHB4nsi6xETeG7YZ%2FpoUVHQH2EwahdIsQ5f8euZJzpi5n%2BK3y4Ax%2B8yOtIYIoLOX6XKfTYKqJ1g8nlk1h2f4%2BqPVudyw%2FafyQGX7HVFcBYfg0%2Fz7ykg5qKIcp7IUMMJGrph78KKlTXyJnDbU6yt7G7lQyXPYYSel%2F9mUx5QVnLhScF2n1LkEMgL%2BM6qDLlD%2BW7CiN8lLwM2aFtZe%2BIECgIRwvQ362pUYpwdBnx%2BQys1QZCfyIsBSza9dHIhlbI%2BWqfxYT87DBryLNWcox4cZXFcwYgBrTMWVrc9V08HprPx8vnnkehoLF%2BdpofAzyuxgB8RnSLWUnbV1%2BvCtNZRGNzCSknWzG2w0IiqCa2um51wzZ0ARBL5lCSDgU7xe40T%2FMadhB370rAVBY0Th81IjVfL4af1ejn4Gj%2FNMLO2e3S9CEm4vg%2F5%2FffVUwvb%2BCxwY6pgGaUwMrCQ3qJA2hsCY0LTO5IjQQwgYx34b6fc%2FiW7sv%2BvDYQ1Fq4DdE5b44Rs5R%2BFoeSSkkxBX4rduUMv7U2krhgA5t66ua4wOBEm5ou%2BwgYjtePBcZadexUnpk7SsfW%2FmHFobroPsjeGEuXXibtIvzhaNlbzzMwEv2BRn8d2kEzB%2F%2Br0iAjaCBc6VgPNQnsA%2FU4Thm2Qgui7my0YIBJqskt0PwxnSU&X-Amz-Signature=e30a3799d2b9a34442e4f825eb29e4211da4ed33f2ad5463d82ee147c0bb466a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

