---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YB42XXCR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9WNCD0EY1qhoQG2JQgSnb94M8vfzaJ3UPrbaEqN45EQIgWWuM2zmIsNeYW0o2%2FyGGxO70F4fGDlPZWaKAtUWAOz0q%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPob%2F%2FhSSVOCZYR60CrcAxouhVehJ9qYueh52OrAplZ%2F8w8Bztv%2BGgs90jq%2BFY0ny%2FZmuLY4RKlfVt6Z4O3wTvIfuSWD2vREN%2FMPDEuKr4XqAqYyHzIY6CEDBW2B3xt0QBj%2Fv72Ol4zgdVwYvLG4f1U8pJZIXsMBh68AhkHAPntU8VdSyqrYW4ofYjdYaV7rG%2F85zr6N%2F1DXBahaea43Sg050zuwWd1oF9UsRvQTE%2BG%2BmoisFWRSFku2I1i7SswmH1DR%2BeXPXauWRVKk4Q4jh61P9BOAPvB0BIVihL61sHATrrfjS14chqhy7W2yQXS2XKSkfxMEx3oskGTG9Du6Mq6WMm7ZMlv32DJDXw9%2FbRqGXOu5h9Haqd5316oSOVMq66Vb0h964TXNuhulZgRu4LZAUroPE%2B%2FE0EQ1emxBXTeb3Aolqrw%2FyMP1c1Y2ilm49TaKNDgTeltYctCdzT3tePzluXZKh0njcY9yuIrX7%2Bf%2FnbXWTuFJDUoKgKFKDTTswcXh2xhKiwxIfDpsfz9mQKlT2AEOBlaAmSnrWF8HamWyCi75AD0hh2tNxh8779032P55lM4ie1JJbo8HuIS9s3o0mu2ZaYoeFSG4gpMQgazON1A7yuPcjB%2Fyri30S2zv%2FnF6KAucmqthGLwnMN2vgccGOqUBe3OF0llrr%2BgR4dUsCPM1xaNrrhm6of9zUKPvqS3p4JMaOfKPiMnjis3%2F432syaTXZsRxAQhjvJtuXVFpXKjvg%2FrRHCTq5JlgEKKZ7lqTnmjE3pCC%2BSA1d92UVegxd9bR%2Fl4GJfZWjdJNdMMlesvO8CuWnv2TiWTK%2BGMAkcBc0x2jw9%2Brp12WpntgnHS%2FQPZmZJ6qvEstRZMWb9%2FhFmBsJzQPpDBb&X-Amz-Signature=bf25295c2e21a9fff021267fd1575d6f48024273ae83a943d455941a995b0507&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

