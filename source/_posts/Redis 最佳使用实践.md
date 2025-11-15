---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY2ECVE2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T170058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDAsENdkW1uJIuGoqLdw8w%2Bypt7CSCroYFqhCmpDe5taAIhAJ%2FE5GrCRGvMrCh2ZVADa0zSpldjCo8mfSNb6wcdo6N1KogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2BZi2FOtzfae8Bhl0q3AMcHGS%2BsXyKmxEvns3Lk6f%2BEJDAPbWZ1QRgMJ%2BOTd1fkejGF4K6O7c4syOV%2B8QJCw9cjKis6d7b5PzcJUrkK3mDVr05xYPZmYKehUKPqetzrw7cITAl9MIpyPBJshRK5qdWf6r71RWZH1LSO41K1rHWJtZcu5yCQkAHdjeb%2B%2B3xZfefGeQYoNl%2Bb2aHM59bBiFjdcteFsqioDQtpqdqoSLw3Z3BI8dl8HlgHuoGERgAthwMNpdqnx0pBmZnUldefzuwJly05nJ%2FOVQwzCwCV6Xlu3mPL1Gqvw2922eWB%2Fa8wUAteYE6pzeVtZOnDiSlH7DgPI98JMUmhMKn%2FLDzjZhba75aXhxjq0yFE819lQOia1Uk2NITM9%2BaO4lei%2Bx96sM1V6yLFoXBVYfPWgubbisAF0HSKBfldcInO1%2Fsggw6FLgQQ8Y56%2BgbUwdBDQANm6A1Mn%2FYeYuanPZA%2F2nPRbIlt%2FgiJSS4aedSfh5Vabt81jf%2BYn3Uqy5%2F%2BqRuL%2BIput3QUr2Or8ZvBs3VU4eV1CJ%2FTKoPS47W1dsoYmMJ5%2FXZb2Hnmr%2FYsGRh5k1nKGno75pLpTZky2xlggQX5ep40Hr4iVxtK19RkSPZLxdoiNCQ351Nrq8uFw9ggQEC4DDvn%2BLIBjqkAR47FanKyIIsCD9xXclv6Vp%2FzLECifw3VlGI9xMQyyjtx8SsLj2VrKg6tdJ3wEz2a3zF%2FNVuafoTR0f12x%2BtvU1IDJ8TbZkCsmeH18yOwHinzefgcC6um7bmTgmXKKQvtcp1N1ZUXz6CPhF5yyiBg6pt22PCDIalZHzufC9HZlxHH3%2BEidmAjz5eXoTctgwKawrq1QImAUtxkZOLxZ25SIxogSds&X-Amz-Signature=a9c8d192623a0f43a3e402404137b71646c8add5ccb7120a173270b0be6ea56c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

