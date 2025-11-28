---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X2CO2CL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBIZu3VE9KuvP8M5k9H3gX%2F1xVWWdrK7hWDfswfuM7hVAiAXsnfdBXuAMLOBLpgvIIrOcfIYjmoo0r5UsW6NUbJnjyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfrWvkCBVifEnx1KUKtwDETuTbWe7ENrBXHvHZFwVwps%2Bqe5urSrqzO2OUr266CpNO6wXkTglH%2BLhDdoi5YI6J2ZQQVZxrABs5INiL4TAuse5HhPleH6SvpZUZ550lmFCo73KXXix1T0Fk7%2BgilQRRCbd%2FXOAyBKjnGua4ZTXDaSChDyx2VbjqqGVmDqj%2F6vkxMMXzzXlOvpJpA6OvAKgsAZzRCZ2LGEZD1JEcvD5Az6pXWSDh2KcFE5TK0TFsqicQMbG44me2GpufZnMBYI3RCVCUjs0zKZZsRGLpN32SWTYg6jaVVuMgO46FubImP45cnar%2FgY4hcDMYg9ZSQUFxMiqr3yrA8BMn0F6L2KDpOho1b4gMEY%2BbXTaMb3uVS0rXqF28t58fgwJiwuR6MAfSYeToM78gHPjg%2FuN3p4GhMMknimHIVvzDOBMhOu3YHLi%2Ba3S2V5E43%2Fn7tS58EnD4yf1hClcRE6lFOviUK6QGyOtKdXxUv2dyaPCogQhi6mCVH8yCla2uBxezaS3hBVbLSvZUHSZo04Y7z2FSe4wBzMlm%2F93eUaRxwMsI2I5nf%2BiM3tgLnxG3%2BNHRryIQljW0ZqfM279KVnXUjcfBTzL2bDzb4sZOWaO1nTmTU424eedt6lJfWLlkqju1mswmrukyQY6pgGFFT2z8bn%2BFGfUJXCN%2BftPU018Du%2B9lFT0oSAP4IDiQk%2BXE%2BNvp3buwNIlwm%2Bb7cJuNiKhZIzwT784XAUWlZFe4N7%2FHoNMocSLbUmAMcHcHuzqlua0OyFZSu%2FWZc7EQiPrXGWjDR9oAKqFb4ap%2FE1Napa2nnjrdI4l8xljOlzXKOwQzenXpUv2zvBLrwp4Eb4hQ1rTapbe07u2jQmeRvEUVH57rjvS&X-Amz-Signature=e336037821e1367677d83a712d432252c4ec318c19d3408e0957fc7e193ab615&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

