---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGBQYZSO%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIQD%2BpEF0wwQ3oSA%2FV9JfXQJyI96hm%2BGXreR6UubGD8aEXAIga%2BBV14aIdyhexOmOe0qJNTspowhExhLwnsxoy8uy7icqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEQlEB1MEnYzupe9SrcA%2BQoLpL4lwpdqaLGGwwuodkKUr343N7DAV48jMcXpniI%2BA66iHTBw75ZCFnwWxJHK0pkouodmGbXh5NiPPyJpTdYpircGWU6%2Ba9mLIDZmNXkENyZyfaxZBu88RFdXMI0THQiDIAfPIVF1ZB%2BshITNYp3zzLTcP2SpQLT5s7zCtmzSpJIxqE4wHOKBm00TENpYcWi26yoRBSCeObs3rdMNBNVLT1OEkt9WB0YnCrqCrM%2BPuaQdsijKEWyWfpg%2FEUgzaWn1I7LzYRWjHQ%2FP5qF7xymjANdFoGlNRVJmYHyXu%2F%2BIQVmZ5CA3Q%2Bh8EzTalD6FpDbXGYFiEpBS%2B4dAqeAAfwb7iVo%2FWSn%2BGA2I%2FiCB%2Bd6%2BxijWXjWP0HTPFuEdsgvj%2F0auPiExagtjPZC%2FlfPXtoo1o4M6MFX4a5K0gxlCAZvnXXsY1AM1VlPsadWA4%2B0E6ZXnjyHKFJmv6flA3np3h2whgba1GjsPH6cFVf%2Bv2m01%2Feae4V6rB%2B0mLCrqEtCqLnByBH0dNXVCe5adywDNriC8ThgzxXseBFObq7cTD%2BM0%2BdSnzyXUDaTzE5vOGM44cE7QzkdP0IBqBjzqE0HaMPRpom2aCZthki5kJ7oeilz1moe3zJpkFOnd1GfMNHbu8YGOqUBrAY3LHoybVA1pbZ%2BPHT7NqxuwpCOVh0Y33Y1bTcTeFfrSwiJw0QEAwS1kgwAfniGEBfZTOwmmmQHgBjoqSJWTJMOaSyD4K5H6NhNpW%2F3HyAU7nIlTeoi239LCBQdIx4yP9sRTpP09hxXUOFocRw8jRcBk80yCI0PrZ34BBd8IRtfuSWgYWRoOcnzPFC7BZd4wXyd6HDTgCtGGKI31XdhOkmmnxwv&X-Amz-Signature=1a24bf7eb019f61ee1c32989a946a78a600bba17f72baa9cc015a5c7f1fc293b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

