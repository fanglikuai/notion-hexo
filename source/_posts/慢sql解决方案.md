---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDVUWOR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQCEzF94%2BbJ1CZbHs2uecEDLB%2BvhEUQ%2BeiepnFgXBaIrwQIgK2PnGuBn48KSrDh1z2aI3UQimlafJaoFAncw10MWpnkq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDIDsifyxpVQe4UG4iircA8ZdTWpJexKfbeKRTVPqQxK%2Be5AtzrGHkAjETSqm61FG6PoSYJFIMsQ9uLmIGh%2BtCUdYLMwlIG%2Bjfu652mAvgoOBPhTbQ5AOCA5ruyqfgQN0PdxRlPmapakFk0%2F0ZKe9Iw6a6AtoJ%2BxCfemER3Qg1KP0d7ZH3kiAF6xVeSoZvMBzDsud4E%2BQC%2Bej8Bvfr1%2FlHEyU9a4DxrUULJcpNPXQz5oVHmJmupLrgemf%2BdEXwulQ%2BlhiAQezGcHgv1HB7gdVwRwHW%2BeJeIRi77K2Svem5tCtme2xOWy%2BhhAiu9ntc%2FqinMXDZoSbhuXxHN0o4pSWNxjb%2Fg8ohT%2FPwLH3eOX%2FhgbsyNz2t0q0UsZlRHEI328kIpToeJnT4dzEWIQCQ5Ih3KVbxQi6Yz%2Ful31QoU%2FZsP2zKc3wQiUU%2FkWtS1r2rL8hrWog%2BLIVQRt0SnE26hkQgwxwBNSxDIPkqT4EL3fIgkVOQ1FjV240Bru%2FDng8p%2FfQbGaQKFWU6LO6TcZQ0bY%2BT5Q7Q%2B8DDNwh3HlvGqJjZi51EHKV1Rwh0YSLEBBmJWlDDo%2F2b5wzQnBrjkQJov%2BquhJmzQ66asdw5G8NR7XZ9%2BFRD5gXojq3IXXzHWfbD%2FBXcOS7gBoYuWzKKV00MJ%2B6rMcGOqUBqQPUC1%2FmmhA2m2K%2FTIJ%2FI621d34zW3oj9gyZ4VMsIFWh3m%2FSXWg6SgLK%2BaHOtJ1K2h4WiQ6m6lN5peTRVwb3kuLKTsSrrM49dFTWKuEv8z7SQWOvtCvGKqFmdGFIGKydWbhlOWWaQkEngjsY%2Bir%2FqNZJ%2BnQKn%2BCZ7pg1KJJPLLciW%2BbSYQAg8kjb7lShJyTvoBEflsxu1U9Vwh2OwSBmiaC8tx1I&X-Amz-Signature=beb41f524d071ae21035e246d56384d25915138d1afa07d2c8481c652ccb79f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

