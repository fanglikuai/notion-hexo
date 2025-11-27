---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVQD262F%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz9pO0amwxpZD0ErKlXdSufwFz3GkWxwXkd5f%2FGYw0eQIhAJsrJE3HiWFnyLfMz41XzJLZ980YN9xW2T1wWXVnbjVCKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBngntsbF7JJMNbHQq3APctIwj2L%2F%2BUCktbRO4tQqzjrKP9t0bhPPGjS6YJs%2F77PObf%2BrLnuoV5RsZaZ%2FH%2BV9DWRxUPrfpywH%2Bd5G4wghE6lw%2FX4L1ffJq1AjdIQf23JSw8QXbfSa%2FbyFqm3ah%2B64Ed0gcwb9YAX32TJ6d8KIhjdF220sYY9a1ukLs9XoK%2B%2FoEW77pcCbBeSt0IjBz%2FGxrmJfAjIDmhOGweYJewfwYodTJ%2BQrqsMSPhBakxzkLzSIMJFOgHhWd09MHRR40ncobH7sBJPIjHCRO7LxtRNnWna%2FbDi9rliM4whmOdC%2BA6qlMTBysBxunVQrKRa5oGfcsevmU%2F6iME1FCT93mSvSRBHtRaMhzItgCB1dR80AH%2F%2BDnmVZ2vSZuA29iJSWVLsgVckqgdBzFb52WFleMyv%2B24k51LPuP3qZlcPHOhSg9plkoWHYTsma%2F01rN3UYs4LqaZWqzf2zqe3cGjiJlz%2Be9mbgRIaXQquCDkVty1NpfH9keERGdaErqMk%2Fk%2BtuxHGeXQChSRf59iFn%2B2xAUqvI21Yp8Q5fFcYmmyH%2BroYUkeOAVBmqb6sgVSCgW%2FZHSCqHRdmZ2LKi1XGy%2FwmsTY2tYc1jZnD%2FyJVXjMFHx3cZmJgMm39cV7twmvqG3KzDcuJ7JBjqkAXgypXtXSdSZ1t6DKVSCeK%2FPNP0JVwenBAkxRbtLxu9J2u0AzWIQy7n5XzOz8RHLryYeg%2Brc3eJ8BHtqLwHpbR44a2PMbvjZ81cloR%2FX9esnXqaGQHoEnYdJwqW%2BCHRvCKW1yFJLkWZPPmIVP7pv%2FciDuCDT2BVl2YHvSrvUWShjBJTGh7f8NTluJiZoYxh%2FAFN8YJj79M9TxWl3IKbwfv4HtWdC&X-Amz-Signature=4bc1ad70f8885750f1b80e3235062a068548d4737ceef79407d0204117377558&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

