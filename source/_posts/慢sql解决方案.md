---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QOO4YDR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCW8%2BSDHK6VPW%2BTt7c2hDkqvsxEZgJotc3cT22%2BfzvMrAIga6MJqOqHfyo0JvoiaaHSNuhlJXPtWrExbpI7X7PeP5Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDPmrXfMer22NRGpmByrcAxM74MabOKtFANVti6xJeCcko57amjW%2BoRh0IpvzVvcQAbaSSKfa%2F%2BrmvZf2hPKdG%2Bx240R%2FZ57QcpZg4RZOEgAZOMDGDv1MT7PETdBkmo5LLKU7lipxiJuqMfY49RsjPA1UCSLtszO%2BpuLUNX7keUjdh3VvnXKgBBtVsUHtloqTTg1ZVgkvAxsDh0Elj0eyVGPkI3O%2Byd%2BjLU3COfGGBLy09%2FIOiJFEUuEq2K2zErUpsmyGaF8OKtXfbw%2B%2F6elqKrJawuJSWpwl4C4R88ba0Od6NcdQVu%2BcC9evgLZuPAge0gFPfXhhCDaDCIzYDyutGB2ysDtuba9PPI%2F4wh5DEImMe%2Bmu7F%2B3MPeT5mWEYqd3dQJMFcYeJt7Kd4iNJaIc7WgEkM5Fv4FTOdBX851B%2FAv1afnKC5muA1TqJ%2B48f0KOxYsznKLrGxSebo4pEN0nD9lIHTbre59t57bBcg%2BVmERcvmn9Bo0eYRQfecd5Uf%2FjVfSIs9%2FYdH%2FUgwi4MLx%2Bt8tlU2Y0kEyZ51E7Wn%2FrLVdlKrBLyRGLdxDru5fI8fVjFFAqxv7YJ4Mi7Mw5cx46ZooTv2fZ21YfPjt%2Fc%2BiOnMY9lPHPKaQhs7wE3go8kF2aeMB5ru7o8Pk%2FOEOBMMm65ccGOqUBLbcCtN0%2F0%2BUa5HzZWbFJJbtXbcf4ahhda4VZy%2BiGjAzu7XqDU2w4QpUmVdmlPVAHP09RKzPIvw%2Fj5w3ZzJJqFl30Aq6MAer44SyOwslvoSRtrHcSmCoS8utbLk1HvlZAGTf43GVRM3VAd8GBENdT%2FGHjflF%2Bv%2BRC48Zrr0l5QGUYAhzJwtkRcWfS2TKqRA%2FBTxNewons2OB5jSpEPM2dXzI4esw7&X-Amz-Signature=c397270869b3304524cb89af34e973768842fafed312c7de5ad864888cd6cdef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

