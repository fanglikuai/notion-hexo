---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZERJOFLO%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDzz5Vye8ywz49iOT0TMXb6N0kswqA3IdpvZzII3u6cnwIhALKaUYxrhe9sLhtLBtIkWY%2B%2BuHPn9VaYV7%2FOm8rXSumDKv8DCDsQABoMNjM3NDIzMTgzODA1IgwcPhiH3WUg84KxGbQq3AMjdwGCHOlWHMLuEayzXVp6bY%2BdRJaq1%2Bk7XYmXZoguCYMmMX4tneP6CI795NQnn2U6wZb7lxKbrX6MtbcYqyl8jBeR7FO9O6ch2sYiar6f9cWw1WhOMFlknZerEj4Dy9YDHZ48du4xt3ktzN1qqA9IxrfPQ1hZEPJDa1j8%2FtyLe9ZanOK4%2FSbWkFPGV7Sb%2B%2FpE9HPQB%2B%2BCNZ2TfEw2mdYBBV9ZECBWwx5971HJR8rqSDIemH5ufV5Rt6tiNByHrGSEqB8sv%2FwOd%2F0%2FhzMdkvHAY8vhBm08uoe3gyLsYyoUuc4OiGdxPiWAzw%2FFDaG4xgEjLPabEwlbUb58%2BmUTkjgzGfdj06LIkX38pVFuLHMCj6WMTDmnUzvbz6yJOFQ7q5uws%2FLlkZRvitkG8wE10DgYYUHmAwxX2azQOxsZvpdOmu4UddQ4fFqid99s%2FxP5KPDuDiz4Rotie5vy%2Bgye9H%2BU1%2B2N9Hk%2B7%2Fm4rIfB753X7dleCLSneIIj5dxroguVMJoEQH5aR4559CHEOu713xJtwEU4E6KNmuDlqvIU7Y4Z5zwZSAVfHiPi6bXQdje%2BogVtg5xtMTO4JPE8kHMqvAyQ5ltEUv5iy15lDUTOjyrPyE99JYd0PTQ98VchGDDG8JrIBjqkAQ5QiDS%2FOOliOn5pPh5H4bqmB6QATfNRfBI0iPSQXLBQfNdVizq3ilvZvZdBkBbzwGpbe4zJ931c1LtVltRVmihvxYMurYFLwzB5mQEEEr3XwWcBSpO%2BEWsNJ2O5saHDbTUGF%2BNmfQbuYWLkstllr8HX2wWjZFQ9THkiFfYyxtKLW9KkbVa2hE5HnVAq97PPbB8vTo5NJrIU8rKicjMpdZCruqsL&X-Amz-Signature=139899001b6830641c6f94af7087cdfd1d6c53de7e3ce3ba6e366e175a6d9205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

