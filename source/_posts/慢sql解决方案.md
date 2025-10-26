---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VL7QZFT%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBHZiYz%2FAfqPFxZA%2B2YFjocH4MC3NsO5RnSywO1LwCy4AiBgMXFspU5qVeUPnOn0tEDaZN2k9POMCqx2I5aRzXv7DSqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2L%2BE%2FpNYyAPQ%2FXEJKtwDKo7tklnLSsmRxsJfY5jO8TBtqH%2FRRKIXq1Sm8A%2BavYznXLKXd5iddtlf0O3P1SlRp%2F4KwMuc%2FelPHM6G7%2BvhL5o1vu1%2FE%2FfsPCTEn1Y9h%2BdJ50XwefY76mD8OZKi9xQML96eJ3aO2qsaybQw%2FLJZ0OqmUKMhXOdBeFbq%2Bnqp9E42kgNCEqWQwi%2FDgmEI8r3BfbISWAwQW%2BApJWQkpendAKdeHvGl98Jph47gbFnh%2BvwzKMUPJQF8di6aV6gd9KVOncL1V3HyG8d0h9f2RjaYo0yRpgnoigBbGQ%2F5LVoUGX04myoFWTcJy3CivdDIDYyMpe86W15zCMJ%2F8mC1fu6VHPq8RVqoG0NjcDocH3tfCXHkuB8whkmUVH1sKme0l%2FyA1Wi9rHpPVZty6UjBqLBuvy6KCVOaJszSy6qsmtxB8VLjvI3aaplLm14i6StXgSCZHF%2FXixnmBul%2FQZ8K8G4mpWCA%2BgipElkVEcYUsDN7o3akjsVEIu5PfYuZHXAGDgLAFgLovxw7chWvQzp64dlPKOu%2FG2mPz4%2BYN5mjMoC7mHHrGdR206%2FzKuLf8IWxfO4A%2BSZ00IcWsqiboTkU0xskpXVMQ9g8rKrmSN4jJQfHMUzlITOxt9ggmsdKJM8wnJ%2F4xwY6pgFDa9%2F4Ak%2BFm8hlGBffXYpNWvTASwCY6AC3Eahtr%2B4ouq5FBguFPNcGRO6Qt6kzclWlOqUILBInb%2FMySfOHaexASKy3QiXJC3xZ1sEL6lvT0GhtbRnaDKj24KeTk7k5gIJQ8UpCAnIuEtk1Pa8QYujFn%2FCKdrMtX1SQ4flrV%2BQQgtypoTOyky426n1B9Yb%2B9mUAFWk4MbtXTJy9iZGshFE4GKdEC9%2BG&X-Amz-Signature=d439cee255ef47c8e5cc53cc330874a9685b47518786c1e3c63f14210bd64be0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

