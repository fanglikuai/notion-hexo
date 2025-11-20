---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZSTALEX%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQDabK0AbR0Pq52f5r50nMBrDrgZUts7KOGwPWL%2F2Wlj%2BQIhAKeewh9gmLFRrN21sJzoKA6m9LJR9m762%2BENKOe83KBBKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz27TjCEWNNiAufud8q3ANPCAaAhfvFSSKR4A8Q7CPMx%2FF%2BcLZaI9sxBpjJPKki1cgB3znmGJrKAQROQo4iwS%2B%2B7b%2Fgt4%2BN6lCq%2FbjB4QpgGfdZj449Xb8BAxdvACP023beipo8SA3x3mWYdb5PcNoSknpub%2B0MDaDQ0XQ10nO9Nr9xu9kzq7MiMDLcr%2BAJH6rUH4aMCZQAZn%2B3deoRBLDTTG9x8M0QCwxJKZ59FXQFopBg9qemaIMjeuH9%2BB3qGt0ZI1zqBn2MFh2VGJ3iAis8Ku3XU10l8WOngbfKCBd604gAY7sIHrEU4LLQu0j6Do0Dfp6mQ0e7ys0Yh2mvr9HVIT1FuLthTRPC1cGY%2BMUI%2FW%2Fi6Hp%2B4vmtzVVDY4KsyuPV2VGyMwFavWX5jkJBVeT%2BCb2NFZtwUA0Nl3zClwv7YKDKqjkvyXR9yX9FG1Yg4rMbcI8KWpL5ADFwWIw%2FBZOGDBWH6zzbEe%2B2K5eX9xuZV%2BxuNiGvgrhKtcDsQC1xfa6xEyB505wxfsfWBjnQ6YrLHBZfdQ5SOxcxWkFMzgidCQkCZcJtFMg%2Byx9QmRPz9o0hJP7WbZ7nBhaRgokDtMYYiMVvsVQ8%2BbqzBm7v%2BUCZl2m6ZOWvKR%2Bl31uyYWfxOMZHIZBQ0dVDk7R3SDC6xPzIBjqkAS2ahj0KKDOeJhEGtPbv2y%2F7%2B7VhmT%2BXcTO8CQP9CFgcyjcmAsmca9fHqAroJxFqa7QCKwnHwA6ixjNG%2Ft4hf2YWf3x6%2FpGHBha1J40mfE5w41UiT%2F8Yu9KSGPyZBbR8Q9dBfdNY1LlsWzGAsQNNOm1qqKgp2tEtj85bRlLl6uW%2FpOGnWJECRcvZOHHFkltPgw0iKqcz9RpeMEE9SpSUe2N1scm5&X-Amz-Signature=88eb622aa1ff52a42f3ac0a61e5c5b64781cef508c59053fa6cbf3a45a3c220c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

