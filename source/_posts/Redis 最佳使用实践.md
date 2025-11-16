---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYSHATHO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGsrH%2Fr2ENmOZmlCA6GcGYjVzWmzwT61eYVt74N9lQz4AiEAyAuE1IkqAHXqNu%2BAmr2JLApotXR%2FmnmTyqBKqPo9nfIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAj98edp2kPpq1kBcSrcA0bV%2FwKX7Vki8qB2OkGs9Cj1PQGyaDhTTesMPOsy9egpSNGAMSOZGnCfMWG7d5YkLC%2FCCKlZ%2FWs1JZJNdBhfqRvS83CC%2Bw0%2BsLtJbMgrmLX33dMblXvaeBfAia47OsSknW2Ggk876zkG9nmZU7HOMy2Hy2BR%2B%2FNYD8PhuAj8qcxUklel3DYmtRKc%2BVlrGp%2BMkGxyN4LP2t6id9zPK4TZKa7xBMRlYrOkrL2IlhXky9evopi7ko6BItw1XGpSnqqJk%2F8ItazVyFwn2RHtd7GAEF22hBTSXxOOVH76wAc9Sjhop2lKYXwkKTyWBALOHvxNoSPJ49NakYm1ZnRgr4cDVLyok7t3Dk0uzcsP3PisrNx5swF%2BhDGocwXrE6MaShjZqGwPDCRI02r77WfZ5r51EmegxXm7eLOjlhbCw81BxrMtYRRE7r0Q%2BBLmLzTX2V0goChB5RE4g4P1nj1osOezVd%2FKlxbUxvJcvx2tQEfXK3zOWl%2F%2FU8h3kb8EjBXRtZ8syKhShRi9chccyCO9O23Im4YJjgHVrt%2Fnc%2BUfCZ6Nzr4BvM1FCmyOyAjMxMApuiS73UlyXM5CzwBLC5rRQyTA3ttVp3IrwgtAZGMEoza91c5NYajsMLVkgKh32D8PMIn%2B5cgGOqUBWNkA72ODGPgzgRLsd8Kn7bBTZmDwcfrBwx50neeyql4KltiErZyCW0021ZrHCnRrcpivZB7r%2Fc3lo88vTquDavkzxGjVrq1FM56IHUNVlK%2BINO41D%2Fq1VB4gnmi4yBn3rqUYXP6Noq6MvlVwcjvgpR3GM5PRmUe7mOVWbzB95J0Vf3I22%2ByP%2FTL%2FNq3cvkwJjsqraC3VUf5Y4HAkQwMDVOZXWhqD&X-Amz-Signature=f2ca01528151719a962ae08c7730e635dafc150240420dfde7c766f0023f8e0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

