---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKX3CXW6%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCG6ul5T12a14Th0wSLyT0Xk3up0YU%2FKTzHLtd9gRp%2BDQIgOvF1A66UgqY%2F1H7R0s9J4hs75Eb84khDDK8U%2FEZR7W4qiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2Fn4plqcrs1AgqwQCrcA44Ukzrzscb7UWtjRF13RmoYL%2Fw1KQL8%2B6Cb7ztHK4A1nvo%2B15CGs91XYgs%2F2ZKtxX6gw705q%2BzBfjEAe0wsB2lCB9q8r5DMCYRq2Z%2BFLtnlcEogX67w0Kh45mP4RKHPvVC2VjYMzVGMJcj9JWuedshnxHcNXuGgErcaCZVtD7%2F1%2Fs2Rcc3iktG%2Bli7Z65xfhugwMhFACSCwLBlvJ5klcXU6ZmAmcYw3CSWBZQzaRMlyUYKy%2BGsGbLB6hDS8W%2FHYjC4Eaq9FrhZv%2BWrEyaA6QPW27EvK7WrWiVKrkxrrZe4fuJy9bVNgkBUM%2BclkvA19ditYx5S0X8cwFq6hnh5WOhrdz9aVvYi6S3y0b%2BuvFooH2hFY4jW%2BpyYUnHDPWAZ%2FUBGS1JzjW3jz5R3Kvc6%2BeeAqjkHihi32i2VJnH106HOEAVPXPUdBjlacvErlu2YHv83LDmLlZabLwwtwIA5aQvUrsrrax4OdxJEDByePO1KKziZ2OxTSm0EpSPqfoULRQDudpfpiVUGvehYDYrtLz2tCzrtYCPynT%2B09FZxGcInHOnv%2FPn4J%2B8Oc3OTIlOw8jkdHObXNeRjkUWT3oArCITZW1I847%2BTzVW3akIVg0cwVosvdtyeHhdGhUO4UMN2a4sYGOqUB1eITAJrYW%2Frez8vMCwwlu5M76Pw30A%2BSucIbS5Yfv9d6zlOCSj8U%2BNqsIOFe00Yu5%2BwP5dXP%2B9%2BlfaHgzGRWOHSnFlVwHOQKjYhRkWki%2BytkThfyb40EmJe0v%2FYIMfC0O8w%2BW2YAAelWYDpgYUad8OWRvAA%2Fdf1afxlOdzgUF70l2cGY1JCC3OD3aTKFhGnYFjw2gGKIRW6RKwO1QoygAX9N451X&X-Amz-Signature=b1d83f69ce6812a083d32a17b0bac20da6d557685123f5d4d9e9e7280d3abe98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

