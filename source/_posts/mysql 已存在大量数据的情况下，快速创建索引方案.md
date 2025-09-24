---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAC2JUE6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYosZC9DhZlz7bnqjT%2BH%2FXLG4b3QrfYcOjUIfR8jnksAiEA5OJ%2BDtPvRvTsxiRwfP76xE6%2FsN0vzno5wp2hWYBcM7Uq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDOyOapd%2BvZCIv1gS4ircA4g7mpfIioWHzhu4YEJbtvTx2igcHzW%2FksmwJaZ2bo2NgYLr6wo7z7spBWbn72CSWGXh1Rf9Ur%2BNz6AXKFuQFNL1960y8p2Oe8jhXgJpGAG1njnw9LKOeOaeziNFJZHJd3YTZVS8eMWBBMMK0MPh9zylz6%2FcDtjJcBzyEGSE9Dbdcam0fX%2Bth3DgQdVjozkB8SqldtX4gR%2FX4POOe2H3mMEJFXIEneGB6XCgrXY9Gvp4MKfwtLbiyZOk6xI434ZaydCDS6NLChIj1hm86Rx129Nao7z5n83FTYNfYZiSWF3EEyG9fc9XyuLs1QS1x7cfWoYIaiLuWlJgAPHncIfgRu5dwWc1izBdoua27VEcl84zx6P%2FTUsprOBeMJdjWNf4r5zyR2seXzrDRLTCDjpceyVvPtMNinmFt0%2Fzbfp21oJyUsNq6rvBqV6pUzQG3Zhjb5fNd0kax9lPU33skQwuml%2FCncWD%2F2DIZcqwOCJDadmK0KlSOZGjtoSxsoeAVMzl3LOvfGewOcy1ih%2BjFP5yeBOwg8TkZ%2FXAAePYiSVPjc144bozF99o63jG9s7pZ%2F5eT7OigKf43FxGzW6BYD3nbeIn9ySmLqI1H%2FJy02wX8tcJx2buAb9EmU1BVX3CMIGIzsYGOqUBea%2BvgRtcStasRqekygQp6q1kSxaatM92jjyGpvkQrFjDS9h68YtShM8mqnd%2FKp31Yp%2Fbs86GNjE55VGe2YWAvPzp0%2F25vtawpRyJFb%2BcYiCRQu6%2F3KheOKeoP0eO6ayGqjtFKdVXgarthCnkuzW7ogJmZ5J9QvwW%2BTNDHSVQ3e8Vx1XxegKSF%2F3NMxM9WJ8LVGBpSOnD%2BnG3JiH5yMpLY6SzTzE6&X-Amz-Signature=a951e7bc99f68c02ad356cef515dde01ac2e15b0ded9efed7ec4558a82bb2d00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

