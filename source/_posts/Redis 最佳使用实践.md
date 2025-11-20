---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666RY6W4D%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJIMEYCIQDDJLx3%2B%2FdiVYDC8yYA4dl2reXiCX0pZSb5vgkm0DcaCgIhAMSENRoTI1t6oAt9lJWL%2BNgLv96eMIUWx%2FdiDBY95cKEKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3jAOKA5g37tOwSD4q3AOLrWd9uqrYE%2FDKZvWg4NMXrKZNtobHraGeLe5YuxKKBN%2FHsZRdRO2pWMOjoRdqI3Nj00I41Q94HJ2QwyUwdbL9%2BOlw%2F9LI0sMJ8o%2FAOB58y%2F%2FZ37yAm%2BDfVHUu%2FkxFeXxxiOoR%2BrNDGMORXf5ICEekV6SvOScrbWBdaUiVcOxIB3yoqd9ZEglW60Z%2FG1thA6Cl5gBywhmjCXYfs7cY%2F0xcqQyI86B%2Ffd9y4EozyVIAJ962exhQpXy3plKFoRgy5juJW%2FvdtjyenzOOeUG28h7UefpLkAPG5LjXEa1JlUTr%2B4pYI7VjVXU5NdZYlWKGkgiZg0X6IrIWScH5qPLKbfErPJW9V%2BnPO8QVIrISt8Dvw%2F4nRkZhLTwIFrKFTtN012PGhQI6EPB27dDgwvNHMsiumbC1vfina%2FDel4lVoMqjhd%2BPyOiIKsHvXhkqmVmluIzvIwpIy9XRhdmyXd9mM0hDz7i1OMzt2469NqV7dBQfqHq%2B23BRoFDG1TAqBJoG2q5deb1Gpud3QQQDmhvyN4e41GQ%2BiJU3lmNPXkL68hOqsJCwZTEMj%2FF%2BgemlYhMeOndITw6vjJimDRQOkwdmdtVFQrOWWFWPxr5Md2r80pRyteidoAV%2BhSegKbj%2BpDC%2FmfrIBjqkAflG7iNGnTF3iF004V2XEAzZEjoiSI5g0qcahMX7Y4QlsbUp%2BKmozBBJp92xPLrJp5cwleWAKAuCoSNPPEfsYBfA3DI%2BPVBX5smHCtocftr4P6ZLDKxHG586WoLN9ZVyQTAFvl2r1pFjdYyl6DA9EulOwEuX%2F4v9kh9jhhHhBntYMGSdB7EpbT7Oxyjkt8A0TEaJWwaQstjoavxI04Hi1oRUaUuI&X-Amz-Signature=b158fb2d80123ed1367e96c81b0d55f1dfaeaee214da49321c7c4589799b20f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

