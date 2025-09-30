---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WW4P36NG%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQDpyjcx2fOwgBxFt%2B5NBsN5g5ds8OFMIZjm9pSh0fDoxwIhAO5qzFoXreHUdTcBF03%2F2%2F1zWffK2ZGnbUX0O1qGg7hkKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx8Q7d8E9RPdtlgo%2FUq3AMwUDk2TWq8FKVIi9HInALS6Ib5Mid2R%2FoSiodi8n0PazTxwsfX6ApFSe8LGZ0FXcttYOJ0VwB7jOyAif1UkUTefjNVreSXKuHCzKVR5wSfp%2FgHOB6FjrEl4zpyz7ueoSUH60uy3OxOEw6VC3hJVgonuzAIJc0tZE%2BnAfqWTj6Vyfyqi1y0yeDTKKG1%2BlmQFFuyWvCUlnfgKdVn0qCXXtu1lOZoCvzuIHJmZzCnYXuAXBqrSgDKFJxEHyX1hBAkhatdP0qiKmIKyd1OlAHUjyY%2FB4DDPnu3Rsbi57pT%2BLkeyKws9JuxWOxUAi%2F61%2FnQj66pT54ZT3ybNbcYBNOHzS%2BtkZDcjRzB%2FvXkHo%2FjlOQdjiWQr6Rw8wUInK%2FijL8c1JNuPA8b2j%2F1M6H2Yzun90WqxDmQablCrnqEiwfqhA%2BogWrKgQFer%2FctLaXWoCG3B5MvUYOT37kpxOyE0iHkZtEmv7exfni1hKxy3XM6qy2dGq89%2FzjmK5wEb1%2BA5GCdafnDB2KTB0XlsfqZIZQ6aRb0v3l8PdF4LLdvL1j9pfzo%2FG61fH7wTVM4LWrnFE9usFpQCRdllsAcvlV6dizNc9xcXJFax9nXp9Y0VrKKscQQFQY9BnOtYa9a7Z1sxjDsiu%2FGBjqkAVt3xXflobQl910OzdV6PY2Mh2ooG4CfH%2BABIODNueXHuPZze9z3J0AtlSqtu%2BeyBEkZkKLymB1feS%2Bq2vVIkMrMQUqiJ3n15cxIqf76B7TijfsHY8Sn4xkMO2JFzX5Qdo6nE4IVrDBpagEeZnlbtitNqW3%2BUT8zxp7wlxZJU2eI38%2FxNsPdozEWmXd69wuaiQodi6amITdp%2FJsMrX3xHSWSwPbh&X-Amz-Signature=5ee16349d9ddf576525c76fb33bc174379254cedf83cd653063721fedbf88d98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

