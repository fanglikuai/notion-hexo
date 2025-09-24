---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAC2JUE6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYosZC9DhZlz7bnqjT%2BH%2FXLG4b3QrfYcOjUIfR8jnksAiEA5OJ%2BDtPvRvTsxiRwfP76xE6%2FsN0vzno5wp2hWYBcM7Uq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDOyOapd%2BvZCIv1gS4ircA4g7mpfIioWHzhu4YEJbtvTx2igcHzW%2FksmwJaZ2bo2NgYLr6wo7z7spBWbn72CSWGXh1Rf9Ur%2BNz6AXKFuQFNL1960y8p2Oe8jhXgJpGAG1njnw9LKOeOaeziNFJZHJd3YTZVS8eMWBBMMK0MPh9zylz6%2FcDtjJcBzyEGSE9Dbdcam0fX%2Bth3DgQdVjozkB8SqldtX4gR%2FX4POOe2H3mMEJFXIEneGB6XCgrXY9Gvp4MKfwtLbiyZOk6xI434ZaydCDS6NLChIj1hm86Rx129Nao7z5n83FTYNfYZiSWF3EEyG9fc9XyuLs1QS1x7cfWoYIaiLuWlJgAPHncIfgRu5dwWc1izBdoua27VEcl84zx6P%2FTUsprOBeMJdjWNf4r5zyR2seXzrDRLTCDjpceyVvPtMNinmFt0%2Fzbfp21oJyUsNq6rvBqV6pUzQG3Zhjb5fNd0kax9lPU33skQwuml%2FCncWD%2F2DIZcqwOCJDadmK0KlSOZGjtoSxsoeAVMzl3LOvfGewOcy1ih%2BjFP5yeBOwg8TkZ%2FXAAePYiSVPjc144bozF99o63jG9s7pZ%2F5eT7OigKf43FxGzW6BYD3nbeIn9ySmLqI1H%2FJy02wX8tcJx2buAb9EmU1BVX3CMIGIzsYGOqUBea%2BvgRtcStasRqekygQp6q1kSxaatM92jjyGpvkQrFjDS9h68YtShM8mqnd%2FKp31Yp%2Fbs86GNjE55VGe2YWAvPzp0%2F25vtawpRyJFb%2BcYiCRQu6%2F3KheOKeoP0eO6ayGqjtFKdVXgarthCnkuzW7ogJmZ5J9QvwW%2BTNDHSVQ3e8Vx1XxegKSF%2F3NMxM9WJ8LVGBpSOnD%2BnG3JiH5yMpLY6SzTzE6&X-Amz-Signature=ec1a2cc27d1655c80a57e1e5004963d7fad0b1446e3a8e28f02000c60b09ea78&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

