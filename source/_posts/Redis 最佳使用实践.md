---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBLEU2QE%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6AE70oyVncerAJFH1A%2BhrCSay8tdT5bDm7wD1YclzEQIgXTfw%2BiTYKLOrlUZTZ4Ml9WeCLllIetV3kWnQgDGlQToqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMEIkycKS1xQRrJp%2ByrcA5dBxJfnf4TM1ansLkjKgOTWwvZ7bpUHaAfMf7gHoElmouFI9tZSkvg3849%2FvJoibfvWeVcCwUzeLLgZ6W6JSfwLtrz62h%2FxEpBVJJ%2Fvz7VF0NdoP3b0%2BqILYdHyOlw%2BNYg9FCpQGIr0GQFuSPNGLJnDFT8H0f%2FN0aFKNtU9oJzxKAHiE0AYNDUBr4spaUy4mEzNc66UAOFwsAVb6VU17axaUG62Y2bWKmjX2PLxolC0Yybi9HEpyYgFLm0sXpk2i3CjA5ftiYKpntVwjt%2Fm0pPSoHoCQapkSQkkr6m%2Bo1kWtVUU6rbnEHYyj7obDuRCnQY4JnliULU1n%2BtpaQxT11fyMy0%2BRG%2FgYlIFYD1xih5CkOQ7ZqzG7dEv%2F4stoV4V2EAB%2FsPlfDr7ZEIhK2bm3zZ6M7fGjzXyZusDwcpjL%2FwSSP2V2dOlhM4KipMBUAbWo73tfJOZZ40H%2FB%2FGWCMkcbqT%2F2jbWc%2BM72b%2BbwemNT4U2KBYtqghWHd8mz1Y%2F%2FhkKL0xZeBrsRbqkBVYqsLClqdSFGPpKFfcmks51d71MGO5Ea9SorZ4SMMiBHidiELwuEYtIgrSQ1q1uoy2%2BaXhu%2F7TaLqrfrLSw3EMvokVkA0RPMQzlapPPBUyhrpkMJOiockGOqUBil6O8OL9I0IprE0CB4VEnct52raWez1Xq%2BOVzLEgze2ii87CpCD1tH18%2B%2F1%2F0u%2BWQzF5%2BualuF6uB2wPrc8u1ioZtRaFldmRamhEzYLxFGjHDC0%2F%2FPrddkOd0HnZy8%2FwJkPaXd5N75PmxviJdacHr%2B3sNv6YXRgVydAnB8VcIzNTJ4cvlyHoDcbYc6UZgSUndlhD9W%2Fk9M78H4lLjNmfLn6p8FYR&X-Amz-Signature=08f7d46ab434b2a53c3ad6ef7a376ae2c3438b7a24cda2c112883bb70639c579&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

