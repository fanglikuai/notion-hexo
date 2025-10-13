---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624QKD2IH%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T150059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDnMab2tQWQHw3vRObY5htJs8mUKLYMdCHhbzfAG6gxYAiAZvOBUl6p3EqR5NKzyVioV6M%2FBSjkBmpXqtIB2e0lpbSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMFxMd5xMxwm94e0rlKtwD8152f1syQvSvkbH4K%2F7NUakhmllFvtV%2B5g8kIHCmtTjMv0tkBiYbOu5TFWs%2FEZJ2lf4VQ%2FG0a7AIC6SebZCXJqHF%2F%2BjPXMbSdQm4EBbuTu%2BwtYa%2BoTzNkXvCE%2F9SQAKrJfMFoMvSUKIKKtM7EtJrumLpncw7LSwQgpszwSQQ0B4wQ8YlMhpJ51z70UExecshpKOpmtw23lPHRqZDuczrLjVKdnbwE4SB4R0v%2FUtgMGPuXK1KeuYXsYqmdRuCR%2FRugD4AX8BjR9OomCpeam2KGUuHDiK2r8PHI3aUrf5RpikR%2BwAF5j1YiLM%2BQaAEtipAaphHg924qifCQd%2BQs6Xfxn5qdg0CSiKFg5UTm6xAD2s%2B%2BhS9dIUNs6M0ZtYuGgMG9fYj6aBTuSc5ZhjO5wvJY%2FatITpLYXs2%2BTUA3Y7JJNjYxy2GVETd8vxwkhNkNCrTE92sK7HPGghoanttIisdfxdUuf5yPEwTlbd88pR2RagZszY9%2BYINw1ZcqUraEOLp8kdO5OhqUGG%2BPhwf5%2BR5zDgNfUG7C7u4Ofx3rHpEOCi0%2BpcYHC8yBpb%2BaHlAPCi%2BpCESsL2I30iZhkv77RPPn71AmYrcRdMwOvmyMH2mImC6rZQpuq%2FtHA6WqeYwopO0xwY6pgFmXxk6dd22A98ysCXSawNsH49nGww7F1y7SZuOwAWVZkdFDCqDLCNc7S3BMi6p1x5odX7T32xEV01TgRH5VQRMEgdHSLQ7UlHnmSwbJ5OjYIaaT9kOLlDOufEmSht%2BUj6ETLElEcVOTlxTL8Sfr%2Bq9ACe%2Fjyk5bhHUN4j8neOpwxvN4BSxkiCPp2IcdIiSy0dXEiuF4ohVY%2F1MUGVq1Pyjof9J6vLN&X-Amz-Signature=8450ed84348deebcb3a18fade5eef0736a3529dfeb523c6a784ae610335165b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

