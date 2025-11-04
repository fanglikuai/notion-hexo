---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GAWDISB%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGX981WJGmgaxQizrYeP8AvhMrjVPTGAfHpB3jeEd8eQAiEA8D003GW5Yr1mQpeXPpPaJmLjIuBKxtoOozojGAtNYe4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKaoTaBw6qF1djd8JyrcA%2BkmXkYS%2BvNMnNMli%2BwtCE66zhEFOYG4RoJxZS%2BAbzCuJqITcggMbEAuIAr4ZivYmpBQuGLTtTs1VU1BCrcMTqFJTdzrYRc6gQuRNXeZt%2BWOdb4O%2By3fn2I0a7DnNbcoCXD4CuVzKFJYXPRouo7FKWoMQ33fqGkLdys4%2BXJ44wf%2FQdyXguUmPAcpXtdlneAfw4mI9ScsGlAY1K6al%2FlrykVYKkIg911gblpmiPS0lq%2FTGfxh8HJiSGspwJsogxqOoErpvv4hjdR20UR3%2Btms4CcCSAi9W73%2BQrey4OgJaWHcNgxB1kCNZ1u1qis6VX93uEnpqF%2BJAq0QGjWCIAI6DowDoBZOvpDPJU%2BnfkZPwSZwv1ch0fPDYq1Lw6J3CKIzustxTkO5PamJa6z0dB0a1EcJ0sh8Dll50bcASZ0EIwWgRnit0WG7%2FxcgxHn%2FkVVf2MirUh5HYHz0GkB5nXMV%2BQn7Stl4o4sJ5XLOseCO3U9%2BsSRl5PDBaHYUuHFoTZNy40oumz2MaWhBMNAH8LUtFE4dRRrNkA7TYMYftJliKR3BE0yNl4G002uHW3L4e1Tp%2B2J%2BczJJTqJstmIgSf7Eo9KTRjXDIzAPV50WYffnF%2FBij2wZB%2BKLHX1KFzATMPftpsgGOqUBKUIST%2Fcd7EVCCgbc2Oiiiy0R%2Fu2Y2zMJQNiWNdveDusTP%2FfpvF4Fj9gT9rjyxbtmidub6XWhic60mZnKSAnV05ZitNY766RUdoPh84tCOdZTqgLsaf%2FAzGH9jZ6FY8yvhIN7F%2Bo4XJ%2BjPj0ys5j0yPXJrUaiP%2B4OEx7DRUyj0P9HtWMNJ7DBrnZYmZtTfy1C7yB9dGWy8bhgaRjPeadfexs5VZRL&X-Amz-Signature=c1e8e836edff5e1a052dece42cd8f9d2c56a79b61108af42df4d1004af317596&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

