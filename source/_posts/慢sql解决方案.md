---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VW4X5VFF%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJIMEYCIQCom59Uz%2Bn9r3IUKr0xFiM2d7H%2F%2Fk025zH8TxH%2FreTRMwIhAMuV%2BD%2Bc%2BNUiJgDjp1sRojmpiPj4LONXSsSvK5SR%2F7PnKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2JKx43gZJ%2FbjcRZUq3AOl7w1z8G80CrJM7S3JNxKxt8LknP8l2wy%2FWNLXZCZZtg%2FJi4qI3BFQAy%2FwJMUjbE0end1VH%2BlxHgkSPGxBDSd%2BrW%2BeHyWwhNoHG2FMurU5UJflKRcYCyguXDL5hFmh%2Fw%2FisTWOVaIb%2Fje%2BcMc%2Fd%2FAYZAjfeaR5LyztFn2ibVf6uQHmn%2F1tki%2BR85xsESySvR0CU32v2TObHpFvQSmWPBP%2FtPC%2FJRE86zFQsJ1vfIer2U%2BnfQD2VMdqtYZT%2Bwre7yROznlnUuA3GPEvG1TWA9M0c8u8shUvNH%2FPs5mGgPJf4WoqCOFzCTFgkgCf5axmjbfxhd8VlHq74NKUw%2BwrujupBWYpRqMcepzkn21VLPJBAyFiInLsEvhm%2FPcDnGHpqjunC1NfqbDwpGpbAi8yTKP8icl3ECsuNkp6ILQK1onJXT1rSioCYkngYHBgz2p4Bbt%2BWOqBbkNwgPP%2FOr9E27IS7a8WF9fJ9lRmb1fyNwIyUcX%2F04zqbhgtigS%2FqXEckYgHj%2FGhpoDrYBlKiyFwIgrYFo28Gr1YQpbs8ED1jHGaN214rhUz2nkrady0DQR3jTiMrXXSlUHLSPx9QMBmX4eGKWqLwtCsQ7egl1%2F6Vlvkk6xyCxp2vYhh7YyofDD54ffIBjqkAab0Exa%2F1xkWepxTf2qWg91l6NisQhwf4mRl2gwhQoqzuxA%2FY9l3pyzkswPc0MDHA3tNN8saBPYDLoLuRHL3nfuebFyI82EdkljLyeMaPJcjKrYnMRaL8HZ4dmqESoY06mxn4qlx4XCwHH%2FrS6YjqtYiInITCogmZ4mbTRykq%2F%2Behz1LpNtP8BfDPeTXQDJphTGh2GWGNyAPvnusZXmcP2I9cMj3&X-Amz-Signature=1fec7c1a0bbbdf4ff29ef1001252c441cad2917dc2502c20c46c486d2c6f7caa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

