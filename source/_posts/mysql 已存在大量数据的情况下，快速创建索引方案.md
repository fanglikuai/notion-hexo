---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VW4X5VFF%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJIMEYCIQCom59Uz%2Bn9r3IUKr0xFiM2d7H%2F%2Fk025zH8TxH%2FreTRMwIhAMuV%2BD%2Bc%2BNUiJgDjp1sRojmpiPj4LONXSsSvK5SR%2F7PnKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2JKx43gZJ%2FbjcRZUq3AOl7w1z8G80CrJM7S3JNxKxt8LknP8l2wy%2FWNLXZCZZtg%2FJi4qI3BFQAy%2FwJMUjbE0end1VH%2BlxHgkSPGxBDSd%2BrW%2BeHyWwhNoHG2FMurU5UJflKRcYCyguXDL5hFmh%2Fw%2FisTWOVaIb%2Fje%2BcMc%2Fd%2FAYZAjfeaR5LyztFn2ibVf6uQHmn%2F1tki%2BR85xsESySvR0CU32v2TObHpFvQSmWPBP%2FtPC%2FJRE86zFQsJ1vfIer2U%2BnfQD2VMdqtYZT%2Bwre7yROznlnUuA3GPEvG1TWA9M0c8u8shUvNH%2FPs5mGgPJf4WoqCOFzCTFgkgCf5axmjbfxhd8VlHq74NKUw%2BwrujupBWYpRqMcepzkn21VLPJBAyFiInLsEvhm%2FPcDnGHpqjunC1NfqbDwpGpbAi8yTKP8icl3ECsuNkp6ILQK1onJXT1rSioCYkngYHBgz2p4Bbt%2BWOqBbkNwgPP%2FOr9E27IS7a8WF9fJ9lRmb1fyNwIyUcX%2F04zqbhgtigS%2FqXEckYgHj%2FGhpoDrYBlKiyFwIgrYFo28Gr1YQpbs8ED1jHGaN214rhUz2nkrady0DQR3jTiMrXXSlUHLSPx9QMBmX4eGKWqLwtCsQ7egl1%2F6Vlvkk6xyCxp2vYhh7YyofDD54ffIBjqkAab0Exa%2F1xkWepxTf2qWg91l6NisQhwf4mRl2gwhQoqzuxA%2FY9l3pyzkswPc0MDHA3tNN8saBPYDLoLuRHL3nfuebFyI82EdkljLyeMaPJcjKrYnMRaL8HZ4dmqESoY06mxn4qlx4XCwHH%2FrS6YjqtYiInITCogmZ4mbTRykq%2F%2Behz1LpNtP8BfDPeTXQDJphTGh2GWGNyAPvnusZXmcP2I9cMj3&X-Amz-Signature=dfa1b4f8a91218c971482db15c4a7c2a8fa21c0ac9d00f272f2a39e315cceefc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

