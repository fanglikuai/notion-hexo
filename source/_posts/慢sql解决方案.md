---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFHGNZUI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJIMEYCIQCgkKUHNjFxZesaljgk2KH0lIThdbQVteyvuZRrA3u69wIhAIxm3SV5I9UK7h20aTM7RYSqCKC%2BbXfQ76RH4MnysH74KogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz1ybjLXMLnOyarysIq3APNyKh9ET7jYD2lmFy6HG4PqlVZVn59hdj6C9fAWgQ%2FXZMvHWW2De7tIRAEKtdme%2Bb%2FIojeOg6xhD5jbqbHCNLOC3MCX9RseLW%2FTgbV8Rgu9Rw%2B5EyQw28eeb%2FlY%2B8zyvg%2FquJl9PsI9kFVJs9GdOvcprWUjrFhRRlWd%2FIC6wGHuvqietJFFx6DFoztRJQ5BCBP7Og434E1pTBI8XHvZHuBAgQE5UOza5YmGT1abrfzxFVs%2FptxmfQ8g%2FqkhlnPLrIWQ9YrQuZfVkqIHT%2FZhXyh6n32h0yLqDvHlqPrvPusASTSk60BRY1a%2FcPcY8V6IHVfubPf75PXWzeOmlVoTBra4vZm5BIHY170%2Bg5D2QL7%2BPoa2SEGe%2FCuaM8jsSlEx4zj9s8nmxqGR5j7TMsJxmT510tBLmMoEfpuaHXl3chn93syq%2FllfbiIOySuMVLzo5jSM%2Bugjwd%2FyuM2Y2jYbG3G0Fjzhmg70c1hzx3rz99J0njKyvRW2I1aLDARzcOxlKx%2BcdQgJQ997AYkR1ANxZaYFRZkBNK6%2FGgwV%2F%2ForGBg%2FL3LHkiCZtfqdntn%2BjCl%2FMwTXQiNEwZjZgAEM412RGR3XbIdQenEyJWb2P1%2B2bjqodPg2KaWDU5XpgqjJjDQ7tDHBjqkAQaBacW2AehgCA1pWmVM5qkDVWYvr5u0ydvc7rEpjBoYPshM1FPKHasGv6eGdxpiMeiJ4kfoSgI0FVoqC6sN%2BOR0W7VMqoMy6gId7DSHG3E6cplBr%2BjCNPBxmAzOysbLTptPkKY897aZMF6lvv2iIM54vnu06iGGnDit9933Q1XV6agFLN0LEpXrBcyUxwLU%2B1ovbk6vUyXI0jbosJEtV1yT7gMm&X-Amz-Signature=fa347adfb0c9a6eee352de991194ce6bba4729e82e24ed625272f8d28ea619a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

