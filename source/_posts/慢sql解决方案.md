---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZA7BIIW%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIGYjEW57zbagRZjpjA4y%2BPbyK0rSFLd3EGYU7cA071SWAiEAnkI86fwh2WVHZIC7F7GhLPJO5E8OTMubuMAHej0H5EQqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2BtfZeH5BG2CL%2B4WSrcA70xYnS7pWuK063mis5Wkx24IFfgl5HmPgR0UISGv%2BdX%2Fp2FgchNmAk9opRr%2FS83tBoPX0U8onAlBfefF4ltAKgo7uyPQI4iqNIS1oBDmZSwGLbLehB%2BEfxaC1Rf24RUoudeqbYqPuS6L09d0bmlUbkn8%2FEqSYX%2FKjFURG8b7IGGqpKFKnjCu%2FnVMvCdjr0g5AKJiYcFJRPD6cwugMBK3pr2ykwBtAWRR6giOh%2FU2O3VjhX4cT4mxM1NXzZU34gc7ZeWKjqGhn4ckULSxLFOfwkk7gWUJniKNlSFnrDxUmScQb60lMH6RlED4ZxoSHMZIAwuytZTiJfEdeWCulWHqzIiuWtVxt1L51000LaNDIQn6matkH%2F8F2%2F%2B15L%2FAJwYOWRwyukcw9tLF406JL4qsCDm%2B5zfuXhQty%2F11s6wyybpY8uNpxnQHR6P6EakjrLX7KDIw1cwGxvK12kda%2FTmIqQjgxmINlIFxxUY%2FICVyeKRaIVY2XsfQHsskCEeR2WUvH1w8ERgTVNtc6KGLWb0fBqYPTx%2B1%2Fw2Gj3cbM50%2FirEJOiN9kzaGlKyzb1YwoYJXlglfH2jNcg8TEgsNgCgoPFgqdfLY1kC4GBvDJrEMkU97rjh5y%2B3fK0udwQ0MK%2BN3McGOqUB6vXfET5Yv21P3%2BqmQWjIOdqDUmcxBWSCtqah62u4CeM9He5QLY0f0lm1jgSW7wAgm0GjOcIIwlFhDM8RqaHmVdvDvjNhL4HlBw1h9IALYCeCUuQKXW%2FH%2FC8HU1N62LAd%2BCzZBE0bEFoQsMSR76DxLFmk8I1guWAbSk8TmYH98wcq%2BmiM%2FGd9%2BbUnQZHrZ3RqD3en%2Fu0Fe%2FQpU7onervJgX1U9Jy%2B&X-Amz-Signature=5962ce93d5d0452bee47a38f273b27a78e839650e7e870f2636dea7548868646&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

