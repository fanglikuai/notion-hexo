---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HTN2VKR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9nJho%2Fz2OjA4EkYe7N91XqGnPsYUyFupjuVduAfB8ugIgAw6rEq2pmvdI%2FT3Z7LqV0RjeqyjUCTaM%2Ftr80h3PsLgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKBdP1A4XUFa8QvJRircA8yVlHAYn9NDzgfakiLVOrPGp6yKR5baAL7P%2FXcihA924rQmnF7NR%2FJ%2BpEpCIiMVFAg6u8GP6ZbCW5jjscm%2Fp05cn%2Fi461AZvY1aoxDjOwGJnq3lRpA6O6diEhB4caf9aoQtODE5Vp1kepApT3gy%2BO2S8EPrTgXq6A89AstoggmNVfGuucEsnqZgJ%2FWsa9f24lp5HAoIl0aEtxKy8knuD01oQLETJmGhzbISDUTT90QnAvrsMFg%2BeRipZndMveEjDe6mjlKjP3zZ8GwqU73dISwEULGXwfagD2sbVy%2B4922J0rRWPBGpucMDqZ7On5lzGw2YLZ34Sne%2BKIOD83Npvkg7HfZOWnf7av%2BVXSs0UcLvZCYXv3aYyhW%2FmuIjY7JVRMCM67fi4XfC2%2B2TyTw3%2FRH08VLGskLNzJu9ESpU%2Fz9miK15u5fVWqOvjhKQZGRfeF18p1BC5VLqO2%2BcIcUgriwKxyVcqfPYC9zHhEnnOVX%2BMpzZgwwKB5fm6mlZ7bVefgxGnI6qW0t%2BxfEUuAcw3dvupLFmwKxdf6SaXGOhLKTTAITZeTCRnilLtx0bcYFC8DHn%2BVkjertISCrYwATZ40sBZcVO4RqCFW%2FaC41N2JiZkWUjyqHgb6Xiuex0MOvLr8cGOqUBecsLPoIoKLwNHheHtrfJ7YvqPj3Z7qqtoziWuA%2FrNn2xOZ8ENKCgt6jiEQ0Al02aCYqE80uzlyuqGJeDP%2B7nKExOYS4Oa4v5IaBRe31gO5tUpFUeCL%2BFethUsOtwACnTPspFOvnnJJS1fl%2FRYXI8%2Bey70CUpmp775eG7zp9%2FspqmcZ2hT%2FrndPUQqaqCzT1sblLRXrBpP2cCMr73QJaBZfTlP%2F4c&X-Amz-Signature=c109426bbb3a989ce3ba3a08cdf5c214e492c591cfce568048eca927a1afb25f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

