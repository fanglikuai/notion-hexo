---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2X4WAB2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGRO0xnB8xDHRZWbi%2FZ7gn6QQvPVyTsQ6ZIdzYHB4%2FKaAiACY5VQjmoEW2G8fMopoRacAgRloqsFU%2Bi7H4FP%2B1WOPiqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIME%2BD8OROHR%2Fh53VsZKtwD%2BVuzUckkxQQNFPRghx4JqkI3l1qnTMUqxD1upmXLypyhejQ3HaZBuYaX5HNTc0Cru0q1fyoDJxQisps69TBK9zYNenHYY1zt%2Bugoqvde78ZPyVTglTWm9fbv8yrgOhiTxC3dL8GnAj4KkrRLMyED0L14wOyFSYNroJC9qg%2FrmuIJzlTazO%2BUK%2FsaeDc6EETQA2YwG60%2FTFj2azxG%2FGFhHOkYZMEJMHsorLUAdQm7upHDn1ZfQm6WsvHsT3SaWzrGLPSjs7vTjtiKRJ3np4a40S%2Bev2W2FN6rQm2gt%2FvTG4g%2BXocXcH%2FKqyRZYZMo55UBmKs8Phyeki02XFBFRUh3PX8GJu9RZlEGk0UTuPmOPQ4MV0%2BBPDsMgrhbMinRMN2lleOz%2B2uOMD5zEFtLq4%2FWP8IT42upMPovyjiRY5HB17Xa%2BpMxjlhrITBmH8e3ysEldUQi7GnHPOLdgCAC7ynru6zd%2BLG3C4v8IcNyk5KgP6OS%2BZagvRcy1mkJCIoKFrYUdm2TW4e7Cl%2FBPBOT10kJN8XzDg%2BEEiuP6DkpTpKLL0o0vPco%2FNW%2FrGUwyb5LUt2p1hi6gkUqHMLmvsR0FHDFVHJw1CautRlV30zABWNJRexQ6i5PZpLYZPuGXZsw94yvyAY6pgGEBD1OdEG9AWzSGGQrIAjByHRMgOViBOv%2Bc%2B4HTa6PMoLcY8uiG1heN7gSvv2RkSelcLi9Q41LpOM4VbVwy6QhWA6Bzxgon4QkVyxWFiN5q%2BufqGStzyCPItsCFLfDZ8k6StjheTJiOpbQOyUB4XtDFuhVf5f8tzfo8WXDChOib13dCv1fupQXarAeDfNEhviIqkHXtKpwNbs83Dyk8RKMszXPJ47s&X-Amz-Signature=8819ed9c2fcdc1224a6139c74f7e7b44699b752c0f0b6f248871f97966f50931&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

