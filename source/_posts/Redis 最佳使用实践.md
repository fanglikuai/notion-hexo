---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM7FVJI3%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhSOJWSW%2BWMvDtpQrusplmv1F0NFFygmad1zrnGlWbfAiA8nWP5n115MHRnjiwkTjUXizngKVxFMfATUvzGO3dVdCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYux0JB0F%2BsJk%2BZSiKtwDkQK53byeklAjFf4GCSJ3xk5fXKP2vviEP01Ork86Ula3uydfcc%2FbrFNvXdjP4bbcPXHKdi5x4Rw6R8rfykNVv3Wv4%2Fl2vvyEa7vfUOOXpyWFQ2YjSYPn8nzdauptIVTpJXhEoHk%2F0Vibs8gUZ1tyA%2BcqTpwXUX2Qu%2FpvMNbLqIh7Y2OU89jgbkmH8BVV4ux%2BFYyjtatp%2BFW3pOcmG901EeTPiHUrGr1nCaFlP%2Fd7lXk15fskAAwd2zBeIS1abaBwZGOCxcdyXthYEZaMwOGvq%2FGLfGfkYXHsHKZYxUg7krgRnHquiyuU9cBY7U0q5T0Ko3v1k8%2FOVD9rv5rbhxX%2BDltfpaiD5ezhUW9TqcAvv2OOBvVmKdXrG7YSep8Zem8ZIv53mWFaNGoV75G09WhH1qPzEgZrvfhHcmoilKbbz26ifMHhHvIkSghYH06gvAUfBgauq35Ol3kH4K8S8jhBdrYgoSf7XfIRN4AWzPJSVV6wfGhch3GqkXAWxuv6ZSG06GgGuKkW4XEHo5WCTpkjJmJ1rtMyuL%2F0Bbew5q%2B4Ep5I7M13FXiEEVCkTICWF3uZhJQd6XWV%2FnaF2HR1pbLtiYk%2Bq%2FmspezgF%2B3wbteoTXOwThkL%2BhnN%2BEYYggAwy4qcyQY6pgEK%2B6peOfXlzbJ0cV55tLTC2%2FbbBgB4C8kTqEz%2Fx9XxKKlIr0qfSOZ5%2FQERzou2iwUp%2F%2B27FjiRiaf%2BgVJ5UznqiJjyLqS4PhEtVEcEsuUNJSOvXziP2O23O1oZ4cXFJWCcfpu2TzzoGlglBRr8znf2k9z9O37%2F7jTBqV378LS%2BFHZV%2FZlgQuVdzitz2DPw2zMzM0ZVGI3zShqw4qeHUuQZkhhTyF41&X-Amz-Signature=a5794dcd5a5f24cec32d902a27d2400da395784d7929c8a69a14d3edb27e9c77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

