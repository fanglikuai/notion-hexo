---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEDQNQVQ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBsuK%2BfsFPEdMqvfab16OQFGAHd1Dw0qRPKJHzw8ZHkBAiEA6aR8UwtIQNuGh02v%2FoDwVct%2B8GNsNYEAkyz3gHRYElcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBgIg0%2FyaLX43n2vKyrcA2Qthmx8dBarDgV%2FLAEkgoCaAYRadqsYGvjyaSUF%2FtBG8Dcc58lx4rR8cef91OU5WzDeASIgSz0EdzDweXrF2NTmq9mgDV%2F7CRi%2FaoHLkm42iJL9QsB6Bebo%2FhQa0h6omkWuADLLI8bOYOz%2FE5j6dbcJ736R3H1cuf7wIudueAMsoabN8cUJwSj%2F5w9%2BtojqJ9qVd3oRm1mCwxKOxG4BH9CE3Snh46iTEfYlF1xC5ai77Q8rRGzipQN1eDlmyh0H31x8i6GwzPZ6mLOML1M9UMeBO3gjArIM2nZf4HGBCeNs0dN8PrFZ%2FKAEQtHNa6o%2F92SiW1X4%2Bq998FZ2jDWA8Ziemw%2BUN7RTLNtxfNkW7RsWhEAZB1I9ibd8xtW4VHDZHPf%2FmSW%2Bqw1VGtVUpvzjQeYDlKh6TnNR2SA1piO%2B4VZxhKZ%2F7%2F%2FTW7FqMBBZFIdrxIGgPI7s0izhMWtVph06osp3gz4jjNSufp4GPG6sZV%2Fasa6O4bezGyoKmfkB8F286%2BdCJFqiJbTUMdCwzF5LUgL8NtVmHcUpLvRd%2FIDPzfbAKmEOCQPSQTIxhVTJ4aN8Q1coZ9eLKHPNmIFfAOzEC3P96TAvhEA0PdYs42M3es98kqjr%2B8P7lKylxMUtMPHducgGOqUBqhQwCzwWEVA3OzVP69w%2BziTt66QOChbkVvgo290cFJQcBWlSbCZAj%2FXEgqYO9NcV1okSriK%2F6LkWrtB%2BsjUeP0zq%2BiVdwr0UyucAQPMBs%2FA6zm9hvpRYcMOp%2B85pcMZ0wteD5lDQUEG50p2EmKl5idpOUyoI62QxBY%2FU6vFoKyHJfKQfcN%2BR918Gq%2BkOhKIP1xkGuVFMd%2FFBs2IKN8XhN00iNNAR&X-Amz-Signature=7c132a2975ec6b238dd89ad73c5fb03ff2b301a14fb74e43b953677169856fe8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

