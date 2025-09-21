---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRXUVK57%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICe4qlfILdN9px4msIaUiJCdW8Kte1yZ7EG3p9sy2k8jAiBzjYIDAefD%2BLZ0Te%2BCqaXniIuDOmbHiRiYLfAR0dk%2F7CqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQ6sNvRqTURCPofeAKtwDks%2BbkVQ29pYn9QRwg0bgsnNSmyL4tDPqPbP8dDjAHu0Y8Oi4L1akaDz%2BRYN%2B8uvPQ3lV7e7O8nN02KpRSrNMSuo89%2BUevmTxZJj6llvnKZhckZF26hNnKLgLGm8%2BfsK5blEWHX5BRssi%2BQNZM5nxslv2J3D%2BepvxEBd8hnIdGoqB9NDo%2BxvBz0YRjWHncaeUq0oWcrrK3VJfHm8uUgbIRAtReOT9QAkaLt4qjM7pV4oDbFX2VIGyjic8DLjoZtiwZBTZysdWrGa2QmG47SQgUsLh19sfxz1hW39hIARW%2BjQK91IKMIO4%2F%2FqhkcmxXkSwS4yjO5TCTqQyo40WhHKyDPDGPBDzrb59KLttpbiKUyBqaKm9i19V7qUtNHTHtjiiQkSOoanFw2VOCB46w4Y6wA5b%2BUN8p%2BJUDh8p23JjetrNgZY2dE%2FEjsEszVZzSdwAxtdNc5cEhYunsJWPalv6tg9UC4NG6gG%2FP6y1twff213xhJho03gbdUMSgX%2BTSkGCF3A2j7pMl1UfyZ28ZLXyQT9rZsardZ8fUVCBTWu2vphjCmDHamMVlHzyl19izLsyZwgYcYP%2Fkv12ESnGKnDwEDYTXbCIZsG3KvbtsxZ5Qs24FObFxyrh6l5Ab5wwif%2B9xgY6pgFnqd%2BwGz5Ip%2BWEXUJbmJI0oFzFEKGur8wq8uZw6bbg3GYGak6Tuuez6KJRfcbGv64ZqVzR2c9ugBM8mlGdF1zkc%2BrDV%2Fp9z%2BqiVDRcc0Gd8NZljcuBXaZpfzypeie7aCqFUd4Ohn1a8UJ606%2BOpOd811Mcp1mFCjD6UjpuZ0FCDbsqJo3SprqqnQbKbsINf5EnXm8tCs9ZWoGSvACcy6WQ3YKRvP%2B2&X-Amz-Signature=dcb16e2d62ef989836627d1ef765b4c066cdb6ad0f68b4f8096e6a6ef01f65da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

