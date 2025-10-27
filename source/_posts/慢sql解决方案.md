---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXGOUC4Y%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeCU0n3Y6533HsA7T4JOwZjKbi%2F1xFbOr%2FUakpeRs6YAiB%2BbYXY3ydOlmxTy9vXf1HUuS7UMZuEJt0w%2F%2B8hfgRfsiqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAyWJ8PynVPY2z2CEKtwDJVjlDnlGgbgDwxGPOmb56kvKGM7YHK%2BoDQ5CbTAeL0xqxrZj7QM%2FQ00pO8T%2FzzBLQGcdwyL4Uj2eTxH6Ilz8VN%2FudKl%2FXdXw1BD6FK6F%2Fu0%2BLjEsRXWqz5eaP0fyxxnBihgJqCPtIc6C4OlTh4DvcrPEE5y8mVqG8enTau6D7KgJAeKIVgygfUB35%2BkItcY2NW%2Btwia8hFlW1jAifmDXwK9kJjkLo%2B940VajujHx8tdGoViQlbQYniLmviutqx3QGfofN6bC%2BzK4Qvc1NNilfhXu1j4f8TBMuDoMexvPv7URe0lpSRazS2U8y%2BfObAbyyGv0ghn3lSGFk%2FlXkWReLdr7jUEaBbexAlC5Ho9pQQ7CNkNWcqUVOgZXbip5WDJq70uOUHXVOZPhtR94mOOykO2JwkcaVNPpFB%2FioFtJeHhHchDalKnvpNczwXBml4x9IRbf99TEAQ5Pys5Ojr7WedcdrKPMSBCWPvdlrvfvvbg8MxaPdsonoQjm0U54k71A9A%2Fqdk9CeUk1mLJ5CoToVbHYAcxzveRQFCHx4dlgt12Ertkyr1nZLTskZYpTRFIqOB8hFpG9OYPK%2BydvDePquObOlXbD9ba0DPEVa0sMTGd%2BxPDOaXPvFlFUPIswxNT8xwY6pgHcTIl2OMb4k1qeqF2XOorX4d2qJEqP16XwOP4GPlWxH7eQFiVIO95BgNLQly8ix0crddFxAXF1OTH5VB4YkysNFVuc8oparSFdRspB2OAZzXV8qz81tAMVrEGlayJ7O4%2Bn7LKqS4RRBQHM3TJzy9NRuR335ZfryTqeWS9%2FosVMvImQkqFD7pZFyzcRO2q721zhL7%2BFGIEvp8o%2Bk2KJFr7PRxUzMAmO&X-Amz-Signature=c4b4af50339690cc1f525963e69227c680e1a4893bd9ba8fb0e026c28db8a274&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

