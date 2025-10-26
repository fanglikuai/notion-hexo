---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5HQM2XO%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDsy7rWQ%2B8N7DlSin6AUHeWJoeqsQqYbn7qdzPpUtJd5AiBSxQ0qi7QcVR9qDLZbUxkRfKPJbp1pFJVQD%2BW9TjhY0iqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO6fSpe3Ds0v7P7I2KtwDtAtZMl1wVOeRLiaPCTcG6VPYpskWt4OUEoY67iG%2FVclPDYtZPWZwPlH%2BChXoFt18Lz4O7mwOfGcvTKOqgM5WilyCxhr7somDoeDtskjQWdXliS4rkdkPO0g7i4hqs3T2P086FK8asmodkrzTy8fduS3Rtz7TvIyj3rlZI5X6o9xTrRUhlRwF3e5k9W6P%2BPdtZym8fp0BzkQW%2BLJfiDGMayLshzCZ8SYUBNPYh95wwZNZZqqzI2chYu5f4I%2FMgaCmEjL%2FtHwDvlnhrHqB%2Btkr3k7BdrDvglEwefkrucMoYvaB1iW2X0u3LhJL%2BTcETidDQhNhwmMkMKKUywZCSA2d5PsgKw4%2Fb2FJ5NoF7DsizUQhXQQxBZ%2BswP%2FnQsbUgJ9X0pUWoUmJOhFyytxOWHAKkquzf%2ByLNm2pfujEr7nEtVuV8yJwQdjs63jeaBo01A7n%2BGvVrxPZU1LJsKxnqR6TYVd%2FZ2%2B9qkGBfVZPGjloTJqW82e5wTSwuMsfqUp2pOZPI1%2B3v1z%2FHDKq5cu4d8Pg2XSk4tskjNPmYCUJPGyfuKeKVZKk2w9xcK7Uu6xXp1WxwjqiuaHNeEC0IpLxB3pARQShrIx1H1dDyo9a61jry5ubPTwKTBmilFLfBwowm5j3xwY6pgGv0stX26sGNByyQeFunAaVKA17MaUuMvv9RZHeEcowQ5cCXOrSZSf8azYTe83OIM2pQjEI6N19B5q%2F2p0vYB7uEFl1tFCnEHiV6Pl2nAIjuzZnOywIdZBW4ucwIuZBt7g%2F57GPSv3Jk7wtXlIj4cOELPH4kdG2qvIS6yipivomXPxzjY0A3WPF%2BxtoBI5%2FTumQODjCYYQWx0wGZ6GxZ2fMz1VtVuJ4&X-Amz-Signature=1ca96c59df35d9387c860ed775238abb36fd2aede4e1bf6487db99803fe2f64a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

