---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YA6VRMWJ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClWOATPUdDaAFqmibBPQb9cVlf%2FhN4R9AKVOUTGd261gIhALIhalhGtH%2FbrisbjthrE6SWbtL%2FXWoRLo1SC4dD1qX3KogECMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxuu4V4joSwCS1WPXsq3APD1wQIeHeIXjtUXkBJGbjl2AJ4mEMrplOSHhA26JBP4xEY8iNOhAOHk7ydEhBfJwttUkHpjq9grtQi5SibGhzdOckjLD%2Bdir3Jv2t1ZJmfXiIIqW%2F1u7%2BeqVeaKlw87PoKszzODp%2BVSqOxb4Q%2F3T0NHgYCwz5Mu7g79o3Ja7qdvsEU1oVTKz6ZkqYk2nOtKFew26eom%2FFKPuEEYicDYcejRacbgj4PnIwr0r6BFZc3ciUYTFIrE53jiCNvY78z9dGNK%2FEzuDWKZkrM%2FATpRaiq8y0%2BVcZy92Wte16O0TRqQsyM73a2KaZInb203xvRaBDplHNFZzW%2BIgjgs5PMOLbiS%2Bn2tKJcH2zLGiu3pg2zRVkFIi%2BmQowQageGrBMeIO2oV9RHtMs1jzOiUT%2ByS%2BaambgCIqnMWAmM9Y%2FYXZl4IIvhVSlGUsTcHyrpzDS4gzh36LcPc3u6CZKt3ixZEJEMG0J9Ud7du2Jv%2B1Y5IBcyrz%2BqXERy5RcJ3T%2FhRvFzl9JePpu8RTxpsdp8%2B1Bgz%2FuOfu%2F9UvTMRm%2B3Uk9WYiGMTPVFWJwMeO4gPdeiqDjqQgCNODfk0gue3otQlwEu2PfVws8nf%2BFbVNzEo1fEUsvzMrsYKWarIl08v46w1jC8wLnIBjqkAR2dSxIMoWg7Z%2F%2FpgiJpW1xRdFEjlKNwV54C4B43VnhY4zXIurQl%2FOvJoI1ZXIkJyV2pm3nQ1KeOwfdvSs5u8X9rwdaZl2tO0toRqx%2B5IGlPVtfSgx617UN9Ze48apgwQ3MBc0oaR%2B68iLoaeh6X6cdK8TvzkshuHGS8%2FVh4eahN4rUvDE3fBfwYngn7hJH4uzo2oFr9dR%2BCPbYGSzntrh8u43ad&X-Amz-Signature=a64ca87e93ec3021a36f08a7fa2f17ece1a7a36947ea028f8d396a1996b47736&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

