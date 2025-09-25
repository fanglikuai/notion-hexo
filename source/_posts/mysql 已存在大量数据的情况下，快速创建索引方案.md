---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YHHYKYR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEWQjeQ0nRndFkXofnupw0PVDKBY4n1qH1Kut2ga9NwAiEAqoVtMVc3BwSVAdRlP2rlyBmo0bWAaA%2FETIMKBIqKk9Aq%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDIhuHWyTxznX5LQwiircA0PXztK8xQdtxzVsn4KIGAqDTdDXnpC86Pn5rRp4P26%2BYIgMIGi4wbzvWKrWrPGXgrbjyib0Wsw6VTCOaLcyHVoSGXEXyzSBMSywqT1Dvkq%2B9la924zAfWSSx8t0hQzrnW6pAidGPHfvIih6b3w3BDhogYBAiLxzxMy4pcdgMftJ%2F%2Bf08EGp4oXCSJgpeUBxlafmXT5%2FWipVT69YPCRILoF8WiH5kwrsW3ZLg4fJyr%2FnMhf806YmOi%2FTIRKbgk5Zva4VXiA7rc5VvwYD69eveDslGpOr4KODrPx%2Bx2VeHfc7YLE7p%2BCn3Cm7QLkccqzybbbRfpgUeORVHtCammeQl036KnZIIJsb9kVDebx1Ybzw5O9SxcjJxA1rUEaogBuW8J2U1LyvR327blQQO7lk%2F%2Fwu7ybVUFsWXdU13x4wCjzRPPeUNDq3htvk%2FgaL3vrG%2Fat9bsHy%2FA7YdzuimaMkFk15ZdQIFXydi14wxlnW2NOcsEsU0OQrJpcg%2Bnwl9h7iPOvpDgBRkIsZMsckbjNg1vSN1th63cc9A38FizbokvE1eOnnTlC64bemMCjIiJ0nmKEOixbzm%2B%2BhN2qvoXW1hE8Vo1WTaQyI4UxHHdQ6EHUPTHyMKqtBWsjYUsX%2FMIqp1sYGOqUBj1RR%2Fj%2FM4r7QofhsgTG64lCOh7f6xa9%2Fvwsg8QdNUVjLOTSOxheFWvr9ayqBBvUvhoUjqsw0VLDgH%2F9UO%2F%2BEXwlVK%2FlfqnImnZ9RzZWu6zRx%2Bs93i5e9TeYddvcbNLo0Yg0l3x00RAd1DDef6EyGKNEXpw7ZjbV928C2kza0TS3BKWc8XMAyL8G1V%2F0VaX4FjQO2lkA2Hq%2FZSDyWmAlmCA2kZLNQ&X-Amz-Signature=04ce5232a3e644fe3256b817715e4070fb2c01f2db529d3f5a13bc6ebbce9239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

