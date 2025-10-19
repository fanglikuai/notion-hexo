---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7HWVKXN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T160037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCnRTSqvHg7aJqxFTGD%2BlFhJAlPsGKuIhlU6W%2BwYZziQgIhAOndnz3kEcyugRk%2ByrCh27jFmNVQCV05lhjXNxpru%2BubKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzJ4BC4GqPEVgDpzH4q3APM41ajo8IyQpwi5m1IhnbAbfBgG6GfTq%2F7l15g8sJz6HpOFzimF9F5ABr75vFztF1DDzjQ1Fxd%2Fhkh11nsPOeDnljo6Y3%2F%2BsgJu4HZgwoRiYXgCxb9XP4%2ByCosOp3slyLTaAoH8lpWMJ4jsHTLmS%2F8PomJ0fQaJZTJ9aUiYKWK63HXRlhEv4G5TCdRhNLjB4%2BDJihCIBmrqIrlIgae5SB%2FzdJupUyD%2BgpiBzkdt696vf3SyXTJTYsraKIMrO94UdplI50fJzBRP5EagALBpKpnxxrFt%2B%2BabduJBUIesffJdqBNuPTET%2FZZ0yfeNbz6lGa3VeRTnafKowg5tLh2ifE%2BLSnOFTO1x4Pb6o1Ltx6Ur3u1lE14IpEua9H4vsfLT1tBCnYAHiHfoytb0%2BwSTDMHEw9ZUXGrG8Rr24h0SixpXNMSF5txirlqgAQ80ZwABcrx9KaR6nCKqCMUORh89xjoGJkpYup6Il8dTxyO4zUBNLWNUJao99rKkCctP1hk4difAA9j2D1tVHSvznfpLoaI2IyJkqnKZHQ4D2kgigjG5UsFCbT2uJGF28r6MBNmykkqN13iwt2XzIjEFxnB0ObEEVdpdQJ%2FGQ1emtk%2F6jarTq9lGpLaEj2Enj%2FWNTCXndPHBjqkAe06u6L9MlQD7fXVkMI3DBl3rr3dWQCpq64m4z2Q0ber7MWrISQDwcXeMBJTa6URcYGlKZBDMuTWPrP5I2BtQrLK5lVTWNMLwUOPu%2BEwxAdl5s4G049sU5LckyJd09fa1d23ROeOAVwbRybx8bTrW%2FLnddeJpDg2ER3TsxZQcqJkV57avSz5Ipjj%2FTLYBlsayi%2FzmgrDpHgeslk8BbB2SMlmJJRp&X-Amz-Signature=034aa451de4e09b15ab4bbd505d7582d19a23ccc30066168d78ef69ec2c7e8bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

