---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB5U35AV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIBH1gmHqrmizge6SoPjvoGPUyIhOVA3Y6AVMUbNf6IEFAiAhgpeS2rNPY4CGs68gT37YcTVo00w66Ul4JpxKGSqqcCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLkrIwIR5OaICxu%2B%2BKtwDLP9VbSI8dZJLWqvF3iK4U8D49fS5kpbwU8s3pt8XNi2IiM3s7m6K1xsxmJ8y84YR6opES9eUoM1%2B%2F1hgz3nF1VXcQk5WC50DhN8Xg2LY2J7TT5h%2F6iOiQ8EvVMK314IGMx%2BIEfrBYqIPA2fGQWZm9zAcYv5XQrXY0hg0Wi%2FVw7gWY%2F3t3p7znTbLVqWuanCPGgngrZRCUUGs3kAILzRmyJCdyaoinhTl0II9JtHz5VcWC%2FG4XRk71xj2TFiJZISryAZVo2%2FayWAprhlFwJ%2B8OBaVOqUhzFv%2FL3OCKVk%2FWJpW9QPRnYRWwWEON27aKeJVJC8RskwomOb8PlOndz%2BX29r%2F5IHpqmeuHmGUKHK3bpjQlGsjGSyTh2uAY1Wonsb082u0DbkZB4ksw3H%2FjAkFe7XIjtpJI2SaHBAj%2FAft8sGo1oB3P6qKEVnZmo7MKpDuH%2BYj3AKe0JUNJxhjM6lHPszthbBfJKJ2OnV3eq7CxuxrA3OvwKrRzzytRa%2F1oGFwyYzT8thY7%2B7jG3eoobNqw6F1KXJp%2BjEl7xHioUFKKIK2dr7cKtLkjeDun71qoMSInOwIE9jIdvQDD5ofxWVdF8svdSfFWdZo6H%2B69QupR8oyHDlbLfSbuXlAt7wwu47SxwY6pgFfC8MwxKIwgGYCP%2BmUUqjWt1P%2FtYZR%2F3pingzSUBJqPo5H5WOzWrQ1th1ObYlWXuENdJ%2BSIA1c2Tu%2BpzQQrVVCRT878NoSiP1t%2Fhjo8gMWJHeSxaTtVmEyPBUo5CDrPqpMC48xhBwNIA7q%2BK5V18TKdwzM7eX1B1XoBBsxTQarBLUE8xmQEcROi%2B91rEgqbToeDT98UWXTnMc8s0z3fsMgBB0slXyO&X-Amz-Signature=f7353f715cd8afd24e4845c9a4d5b078db4cba661bcbb5b55d9eee6cbbdcd753&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

