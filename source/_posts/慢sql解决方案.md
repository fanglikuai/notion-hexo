---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZG6SUGT%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEkU5FXKLRKGspDOeCft%2BpVlhfdcj2%2FFq7XJ9b0AK9r2AiBA6dJPo9%2Bk8OwiT3fJQRtSI7DILCFu0Y2bSShrdEAHKyqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6UW3KakfPTX5fpwRKtwD6AlCe5k%2FW4efehCgNYoFuOd7ljIt1lRszFmpDmub1KOxlCQxmkgt%2BonJV1p4zY0TZidG4oTIzhdO6kozwGNh95xlc1jCxDw%2FYbi5k9ov3zXDB2iyTOAiE2reIEL3IkzlMAq6QBMv380u3WPjqZtl%2BDELByhBnPuKmiTfSCNduFi23uQ4K%2BGePF3eh0sQkusLGGeA%2B3HeFbEKVdsR8d7CiZzhRvR3etrK965ZZqvl1Z5qddyCRn%2FMCCT4lV1cHq3Z7tcmSd16JT0IsgNTkPRkdIq7yI9S8amqecl1Au2P%2BCq3ZreCzjzRx7wm4OWpomtpRLchvyHQRLcb3j7EI0D54%2B424Si6OFeqGv4%2Fag6iQtsuGGdUuNg%2FdIQCwvuVA06RvrKemv%2BY4Es3fHLYrR%2B9N5iJHFKl310L5PKXNcWD%2FZm0SB53Zmv4UuY0XVlj%2FG058ZtItDmJ%2Fs0U%2BnIH8OKwPc0sTQnh97jcok2syI8fx8bic746Mc0j%2BYidjEJZ7GL7s9356QLhFg9IXfi5U%2FqGAgtINCKx8DsjbWTvXAjM%2B%2FGoTfebPPHjVm2kgwHfl%2Fclbd8PZnreC6%2BAMTDpxawm3CyvpqnCHpPaVT4oWBAGW34TZky5%2B3K9Ui996gUw3eC0yAY6pgGz86G%2BCEtLHhtWqdJoFllbtKRejZ4pygHTr%2FtehVpuikEKalTgJGCLOuLew4CCaPE0vyhByYNTvDlPEud%2BGDXJTkTJfjT3345uZq3pCSvG3Ea7GHi8orP75CGxIAwjwMyjOCQMrGt9SJbKqZ4ZkUAxnRXEkwTW1Z0VJ29gFWbQbeOXGVNbQIxia%2B6F9RtEc7Na5P5%2FDLSBtnekpoFJGUVNMcPwnIoU&X-Amz-Signature=315c6edc7c9c82eafe0e1f3f1eb00cdeeb67b0569bba3e79d252bd28e1f59ca8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

