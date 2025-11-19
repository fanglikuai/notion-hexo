---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667O5SHYQH%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJIMEYCIQC1Gdomf5Yhvzwu1xRJ01Ip%2F55RV6iJbBuF4kv8XgoDEwIhAPsT%2FG467da3gIYVf%2BWg6MVGTZNHCnLW0lFm%2Fs2o2qdeKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwkAsHJuZ%2FEsAuXKKAq3AMw2mv%2FM12nrQgU9u75fNRxkkFnDQapN2XB0Xqgq0agImDfZ%2Bc7%2F8T0iZEChwHyhn%2BzuEwv%2BhgWa2mzbXOv6yP02D4qKsvadPbhc%2BzT4LC9zJq9Xwaa8J%2FqSv%2BlYkRFtNtqhpjp%2FZ5h59pMy9hpgOMfIXRp3Go1CNg5ig4Sds83gBuwLiCe3Xs47fnrqMjboletExDregBDiIam49mwRg56HmBbea01b0zp6Omqaiy6GEcQuTxmnnj3%2Bf%2BCF5QcB09Jsjs6UHjawVteZ7ye0J0l5dYm0dY%2Beb3C2uKSw5e%2FFZsFzVHZXLJ%2F7MN1iBbmvhtyPrZvb2PPsrzqWEKvuo2NZVyk4xHIJJ6KmW%2FTCzOVNoQMXVppW5rSJYuAXqZnLF0%2FrqWT83AAgUcXPA2lo50t4daXyhHS9Po%2BQKs2yT4327f1MTblPKxf45joSAQKmCYdTbMylomMEUSqsaiV0HqX8IO9ieokPHojdConGB1mHJfUY4ILP4JjO8Oxp%2FP3CIZ4Ibou8z9lumvqkpaWBqZEE7W7xKHeP8MdCfgppE3khoAZzTOlSiN6h1BeFv3T1%2FmhkRGA9t6r2SI2BUcjD7ehZ%2FGJ17Iv0hU4g0c2PBiUXDmNtN7aAqkXZMYp%2BTCy%2B%2FPIBjqkAXCDJRh6RJ%2F4T8%2B40KN8%2BWpezrHdYMxlh0NcM8zProWJCOSu%2FBYo61aIJKGntqS3FsyUGN7yJ52WJmlcl38JqQNt4oI6S2JT5P%2BOZZjDmaJ0tbcR2crc%2FKEJsBUJxgypLh6VJ%2BvMfFJQbYM%2Bq0FYtodenP%2B0p4bIgIeXq2mB33WFl2NqZizVJJVpvvY81I6luAmAcRK5t2amPmCCVvqB5XZbqEUx&X-Amz-Signature=77bc8e8d89a4c2a3b4fabc5c5a3a361de5b65bf6df37a0e0391f766afd4f892d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

