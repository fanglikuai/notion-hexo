---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3QK5TKV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIFuqSsBDCBc9%2FhWpUsaAsTD7XOuhCeu4RfG8WPwPmzGwAiEAgMdgWuGiOJV%2FSaKFwc3MdSpAUO8%2Bw3a7hwZIPaYGpncqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDoYQMWGfWiA7IwWZyrcA0Ni00TGWRwdZwHk5T308SyIjGAbvmjBIYSK9MOrdZFTqnShhURdGpYFZ3HIsAe87U5kc%2BE9JHFF6Ld%2FPwSzToqDeM4h3bf7V26jLDpYpwgPaB2kiuX5%2Bhe2%2BMfpSoYZO0WjqjqQ5uozbmG5OLRn4s8zD%2FeUmxsPhd1mIbcxrJfsgi%2BicFJf6EqAS8YD1mJq28bG9wJkSj%2BdC4njPPwiK869UvqNrWy4l9PflZmMyCwQ%2BnXFZFXq5uOLdSgVvsG8qeZTpUIqQxwDZZtXnNJJDxpO%2BEL2poesaqUgoMRNDIhNptv8yh142vHwSyHQs98sc7XwzsN5P1DH4za1t03QvBSofeVC7AOcy7EzccUV0WVEBVjlIrD282yuV81n%2BAbgK6K2H96lLRtPjL%2ByPz%2BrkrkMkwAwKOuLf5ceGL2Pjynw14hPWZD4KqLUw%2BQINqD%2FScY3tkN28NQusditW6rp%2BMfQ2SB6Q8yefCJSH7GSuwSQiPSE%2BCHrIViHxjVmw9hngPZYrUhqttq4c%2FH0FATW%2F%2F6XLlwj2nf7LAXB5G5D79HcXP9nZsGG3ZRzTOclEZxgfx2YvGuukmaJPq8YunzDoJxGqqb3Yi%2BS9MA0r1NMW6oXMH2aXsf6XF1DtkTaMLWc2scGOqUB4585T4lvUwqqqHThQ6S1MxyWyagbRZ940jU9q6TWePswPZURJD6IgL5nDEmkBgcKmgTwcw2KA5PLbTfPtLPUyA3Q5TWHJMlXoJx%2F%2BSulPXirEsBodK%2BMzf1%2Bl%2Bg9I1eoN0SE48UVez%2FQgb%2ByDYYKw2Jlks103QfxVY0jU8fjpHD10ilUedW2M7VBfF4S3EFqz8KrVhfUJGYw9xOHV4kF9Adz7muG&X-Amz-Signature=fb1fc55fb47bbfe094cbe458944a692d41d4deb5bf3b43da37ce8e9fa3726448&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

