---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5THXP7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJIMEYCIQCwjLCSFnqqouDLskfWez%2B17SrFfj%2FYjjFyDzycVgaE4QIhAKtVPw47FxB5XXVLlQo1OqhgBJ0wul11M906jqUhGeE7KogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxvitD0760er2noqlAq3APlxiaBF567IWiZEvPg3XgaFPLgrX6lGN6XADl%2B3174KPKbmM%2F9uNmGv0diEi4F%2F%2FLKGUeET%2BAy28%2FPG6%2FQEW0DOpCuBaKlTFcAqC32NlNp4ZzKik5EItDps%2FR1n2w7ROCo3QxsSAMGLs%2BzDm0F%2B6rcnQg4ql6YgWAMGpgMzBGBhnbyl%2BMBH5r4JKkk8D3%2FcZ8a5%2FEvJ1CMJ%2BaER82F79kJnHCnFwt8yXa%2FB%2FoLULhdNBwv7MVyeG3CWdGFevPtjxhfRnpZSAJKEaNJYIkUFNCZyqIjsbTzkY7VbX18qeWuI%2BbSi9gpbW%2FxtFrM60P8ewkzwHtxaYW3zu%2Fg%2ByVtdpA%2FbQ5eR2CTfaKx833%2F175Vnf%2B4fb1Ps%2FFUHTdvciHq27O4y5NYYI58mxWDpJDPjsTiH7S6LkhGT%2F2rmyDnOqW5JjGUAjSzq%2FzkPM%2FNn%2FDEbQvF3S4eOs5ol2wEBdUu4s7QclNgAg3RnCtZTGJFai12SS3Q7f21yntwcGgv94slp5W%2BT4WtRqbl%2BEs63nj7Zz98eAjIGxWSj2evfs72x9DFVMcuf057jZmkxa4LnZV5dPPZE%2BQ4xpjEph5VlL7gi%2Faj9YItNwz7SCGe%2BBPKdYv1FhHIP89xEOdKf7eQjzCooKfGBjqkAZ2AnL3VxmomO3rYGxqUrBoooYDIYmzEJWE5mZ%2F8Zlw7gIutI8SBfnOPLtbYWTq78ln%2BKJLR9Gm9zi8Ir3iw3FBZB%2BCI6nUOgecUsnenHXHiQ%2BCYLXTsY996a%2FHWPXV1hxbQKML7wU67O6BnL92ILN2Kxq3yHvhOm4Gd37UfvgQkWELFuT8sjNWE2KoRhq0KyEiNqUfeMIm%2BIxdAFS9yGJ%2BtyiYW&X-Amz-Signature=aae7d8a6d71538480c138a98eb087df80a52c9d0bb884f599ed5a0c0c83b4b36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

