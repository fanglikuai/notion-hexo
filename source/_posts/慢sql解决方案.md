---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666INJHI72%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFkclEuKJBSiF5MRbi3V0iGlllkkkYKaHHSKIq8aliUOAiEAvHrI%2BH5Q%2B7GVc4GQ6oZC6rA1Y%2BiVtRqGUNJY8l9chUAq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDA53PoH4DrUx0lgXoCrcAyWZq0YwMBOtw%2F%2FrHGAJm7uQnuG7hwn%2FHUV42zd1%2BeF6nakre0lPfI0NpNP12f9FoN8D02gmO%2BRfGxZLyyLkkrW5l3pmtXzczgVieAo42bO53O9sJI1S1uujUrFiKFmxrexlWRHvXmAH8vzKfs51JoGwyQzQHjKwxQyE4xLpWAovw7HvCnkSSR0VXHh8l1Ag7yLVisj3Tw9DfGBc4by%2BNuhDPbHUoK5uAVW%2BV065sSJh6WdjES6pN6bBRA7mnaXQNWDPSM8yv1ZxoZLLAAerUJxmk1z7IMAde4HliONU0TsRS59Avcm4k2n3w7MbMaxP%2Fbw12TDZbAne8FFyZOl5bNBmRIppb3A%2F0OEsea8P385GNSfDQqjtDwgbFosEm9opHyrA%2FIkSTRZyX1Dd%2FG7MlI%2BnGxP0k5wMbGHo1cZ6GCCfdDColCs0fUDVIaHT%2FGFVjay7fNmXCgrkyPU95pn1ICQzyfJyVcb5rN%2BVBVFY2rTJHfAsIDbcIEwka7ytLb6IxmLOm8QVxmmpKQ3C%2FR3G%2FQgnYWTdJKZMHOD5O60jqSc%2F%2FmADqByKgnodDegPYvD3k6NAc7nChyCdNdm2f5VbrWVHP8HT934bHzNsYAajlx5ao62nXWR%2Bak9ggez4MLiVmckGOqUB4zVptayZsRW1qWu67NbnLcpMUW7y0A%2Bb5O86q8lNigZtIsJp8ZNOvYkwkF1Aj6JSDzaw4vHIeQ9vm0usLE8wIxSFITfQjlL%2FlR4o3UN7WfoxFhSfN3o%2FCebSwJB8PyhfBt%2FcxGjcIq0IVAgXG%2Fip5b5Zwjw8LydVd%2F9S7dTUWYsY7ObbTjKRk8QZvBeIswmXio20ELrdls75jvjaSQxiwpnJ9UyA&X-Amz-Signature=394718bbf1353a94cb6b12f58cf8369f5a87327556589fb3256177ab37916bbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

