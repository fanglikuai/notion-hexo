---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VE5EXDVQ%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIBKwC97a%2BFyV1MwKc%2BoTggxOCRtcgWtUKEJPnxEMilNDAiByX1AaJZus6F1DdxLtJ0l9wV7e6yF4fvOSQowdZYk%2FDCqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FyPoTzBcm7MN10ZpKtwDHzEkjfvrOd7Ota2es79b43KKO%2F3FnDfsFJEfzqJxwjxy2S1xpfRTCdu51qLwtJvPTuNmcq8aiLO%2BPlevc91Kb1mXP7KFQNYShfRL%2Bu7WMMJCJQoW%2FUGEvqpfLAh5B0F%2FuNy2K5WNY0m9VZud9sv9Z5GQy282a%2FhpwO4VR6kTd7522Jf41XjSkipYI2eZcH1FKdScQA26vQOAOQU9RrE111SELvm3wXoi7B7c4rbnnmENSNPbXt9%2BM8IewmAAIpBZormD0U10YkanSVlTizrxWgwJxV%2BwxXLKrbiV3ZOfsYW1V%2BnwMz4prdwvXbewPRfnTJsRGBPKJRHgFa5Sn3yJd5j6DnzOnNsS6P9NPJ9OXiUVbLcJ3CTfsG3SBntOpy3ycYCIo8ecPGehk8rq2Jn2Nu0lb%2BagJnlvPlxyxg2Vgj7rHzmbjAibqmr3LBUlDuy%2BjDeRF3oqv4BoTWCiMAoVbSWm2F7QpWsLvRqFpDTJC%2FR%2FV7b9%2FFEogAehvzv%2BpVR5x2CZ98NB2%2BlYAmEAKHguWbQe9zIMMWNuhWPzRcNwmPk6GqEV5KcPVIrFO%2F%2BmTUINgG0%2Bfq2hPn5CLoVYXopfMmBUPO49N3JPuAUSHv3%2B84DY14GS2zCi1AOf%2F0kw%2BtSrxgY6pgE7g%2BGwyQ8mieGacKq2SjuvvEI0yRSVd3WX0JlsPE%2FL%2B2oZXjUA1QUE5rCRa3Xrjj6vTNT5qsZgQy3rB2mEhd0qShKGUsmCgaYfQ0CQIn9UTGBnIWLPleg%2BgtQuEtWi3Z4Uyhah1e%2BFGVHBur%2B4MSXl393tb1QCnUZS0iJvYF7kXNMseBUjSdQfGq5r1upJ9uHfrLCrv63sv8bD2DUdf3SpcZcgrZoz&X-Amz-Signature=4f473cbc5d543dd776099b08b0f4549b5cf5b2736db56c1ac0de80b2f89e9bc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

