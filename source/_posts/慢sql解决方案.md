---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLCDBYK6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCICIID%2BVv74%2FArAptGoDjzupJuA%2B5k5n2PDDn5F1X4Va0AiBMDtZDDyhYXPNd66%2FyowDsWrGcpkA8p%2FMPHmMH7GhVJCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHtaGREQuekLPM6Q%2BKtwDrQD%2FU5q4sQghkeqEv%2FuN7HjBZzzEw5E%2FtYCJTkcWShOnzVdeZL4D6kiugBmWhfmOuwVYwDNktIiQ5Wo%2F6tjUBX3BzrGrVVJHcR%2F0255DTv3i1jt00IYuNtFrZwDHIltygDVMynJ0g5ZkEgv1m5qzHv59HEMGvIvQ%2Fz8Wt4tD%2BAGEeA%2BiedZtYfy%2BsSK%2BJm6V9CgheTUOoGkoKzc7TD0t5Uv9OE%2F7Etqi98xNl2FtmlMgBZs9FqmfKw%2BpB2E7NSOza1wxHG82owz5ifeunQ%2BOsY6HWPVT1NuAFFmj5TTac17UyvOemJMHIyUnYGKVPlUnXOVZ4iMIfkGZuVwOFIPsBpHT6J52MkhogLB2ykK%2FjuA4wn3hzsX%2B%2BGAqnkvpOiLbrL0H%2FXoE2mnPGmj1OSKwXAM7Faod4y%2Bf8M%2BO%2BPPJHU0z%2FgQoQ3RrjL9x5URnblTcuWmhKGyvDyU4X5zUgMcWjZDsCCMl3l4NblAi9AeEWlhhDaNbmss%2BcMtB%2FouDe4lLF0ufR7YCEnQZeUjsypu%2Bsn4fhevME84sIjXGupLYd36c1FMnP%2FeFlob%2FCQMJcnIDmZ7sztD6gIn3Bj3iE7UwIo6MA%2FTa%2Fn8T8yYiKBD0x9wypE0P%2FGqVxulleW8w8cKdxwY6pgE%2FxE%2B5hBY607kZ1Xr2ogJogLFdTA8Al5BZNehE6D5lVgl0H2mPvE6kFLN4WWRgJFU8MfcGIDqpnrjx7P6vpCUkNo0jXmXpa18YYVcIqwXzSqQyPc2cV9K%2FaYUcC92dO64JR58HT3fV2zK7wcPiTK03MwnSEkMOsXH3C68iJWzJ%2F01V%2F0YrkTkgh9DSmzO56AE8ukRivCVjMbo%2BZ3237EQ%2Fi8aT29Vw&X-Amz-Signature=b5307a0de4241d50a2d2cbc7988ec1d6c081d64d39d9b322a40e2bdade98cef5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

