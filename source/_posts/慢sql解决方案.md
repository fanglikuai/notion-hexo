---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAA75627%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAV%2BZBycphITmq4HHF4izO0TeDY1YsRR5fyvQnJalYcAiA0ri%2FEoumm1YDGNP5WzbCGkuSSCSBgiTHrevMHRSW38Sr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMdsA0nXR4l3JvOVssKtwD766QjXh5zLmKmkxB%2FfRTq6xynmNlf9WC5nR7UrN7Ih2Vh4QGBVwmeot%2BAdC9coQMQfkEFdkCYTkxvG8AeaoeJShJTSaEOnZjHMklgACnDts2sG%2BtoOEX0OEnqNa%2BLBu9KFOvCSRiydUEEuKuKxxEUe%2FqmiYLJSk1Zhzw2srmE%2FcjoUb0iy6XtXGBe%2FYoCEft3rrvqtPVwh%2BrBNzeDK%2BPUfRb3ZFF5KCaX05Tyo004mWfPHoYqSMtpXLEF7XSc85900XRemqHV9howqtVrP4jDIIlCuuCi3g%2Bbo8hp9Aznp54ZVhUofd2NsQv8wAdIfuHJp2RRyGrpmE1Ljd7deptbFCBxuqTGi2KsLvC7CtjcwUE3bABfetPZAr1b%2FHKRNmFymFeBySQj%2Bdm7aRKOIvFKdgzbsY9SVOpph6Bv66gyYNockUk4w3QTznuXMdPkq5Ybme5p20xjk4%2B2c8xW1vqhi6%2BkS8R2MxZbog4%2FVgxo0puxCpHFmn9TwLXk2aIBrqvsbTcJmgANkwGumaE5172BBvttlfytn0HBmBT%2F7ptEzSY3ovX4kF8pa%2FGJzrEfrv9a%2BSQbQkDWgyRLxUNO4un3bmAFsiWsTnHrPBu1HgRshGoFqcOfcBIgYlMxv0w2P3QxgY6pgG1NHCgwqoVYhwvh2A2BVwO%2BlGFrX5GjXbrrNcZ3G0GXmLUjWUaRiX6Ce74WrAMNeQ2MsqKbOgdTwI0%2FeB0kDnou1QLyXZfCz5W8iLswa%2Fv3yzIdLM3M1Kt0qKowQQVhUm%2BAlsQ7v%2FL8msZ85M%2F%2Ff9ptGu%2F806z1PkfdA%2BmyyWhAj5XuByWnQ7pcw%2BoXnNeW2KSC%2FgjuulR2PEZfVltg8lauvQ8YW5o&X-Amz-Signature=ecee6de40408ddbbbf523db144e8e74244f49cfe15df960f163787e6fecf3064&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

