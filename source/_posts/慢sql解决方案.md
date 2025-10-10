---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KJU7KDP%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCICURU%2BtS7MA6c3ki7eFwWWs9EHa9jQnv8hUZE3a7jsqsAiBmNMhfe2s%2BuANCvUH2dz4LPHiu1dmjg1ASJ4kCmsyhxyqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhwW4LoZTQIHYFwklKtwDvWstsU3G67zFA3q%2Bh7TZGQmyqsocg2oiT9PzSuK2ynOLI8cVgyJKQO%2BOksDFlSdfbnLBLzak8uhxdAu%2FVTCiTbw5ZkXfo9hLJ8PuPjkiPmKRrjhUo%2Bjc8p1QZjmyBT6f1Il6eCLyEMbuP9LCG%2BdMCXxTDKl9uqBlaaTZaVGN1IIlq9HGec5G4hlkYkDE116CjI2kVlIY60l5te64B%2Bk36IBPsXRNvC0g9jwTCAHWldmf%2BKC8tyq9FA89JFb%2FAlhimaV43%2FbtT1misdzOI7ePIeT5R1blP0QaPg%2FBRFttcgZ4j%2FtFUeH%2FX%2Bw0UmyAK95qnZvswqFZHT8DpdYHPkSph4yeQRxPDsuFdVMJx2BqU4zSK70iCrlUYS4RaZbn%2FNP4TW8WraXTaza5Ds7v%2Fq%2BiTO4CCtallRuOBaVPrhor4qiyCdL7SIwDS594gRLGCN0Y7%2FSDUKHTyCLjrUXyWuuwtJeQJk574tlT%2BtxIHFoK%2FozptsL4NqkC8QbdVXp8zjPGvFqwKP67wT%2Fwn7cu9Ay7TM0tn9aEL1jWyyGWMBvBirZM7GTDdKZIm9l2NBkJb023RSm2yH0HXnFeuKhToP7Speom6k2YNeiVD3fQY4OJHdY%2BFpH9ghZYK%2FgOC%2Bow%2FeWjxwY6pgGDCBuPM1RyNE0Co58zgJiT771rDYzXKSqAv7mVgUrJ9YzFvFE1CjecDQGpH4Sb4un8oWdguF8b08ylaqFzbmtJb1S97SDlnI3yKVD59C6uzgS47L4mra%2BgT0Ase4hOhVytJq8XeP3blYj%2Bz1qwGUmfNn7OfGpwU8UYiNKPY9qsno9%2F3F9jyMBaLe1z%2FZ6wuZqERWkcwCjlKEMX3THYbmq2p6%2F74O4G&X-Amz-Signature=543a5be2432aa619069e4c119b532e698baa56991c8d1e68481b2cc4f1d113cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

