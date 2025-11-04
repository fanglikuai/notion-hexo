---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZICLNY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvFlrPUuv7xVGaFpCVzqNOia%2Ft1zrWdEPcso%2FgdqO6xAIgSLmetP8E5c7KrxP9%2Ftxaq6TX9RZ2pNA0En%2FCbTAa6TYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDGqbwd3OdvtlplPxYSrcA8vPSajzN6Z5sdE6fdPpt0JhJ6XBlcpcylXy8FnvNaxJxCzOwuFpNLCdjrCx7D8g1AZapjyGYOitf09ZVUGI4dRcVTZ37cxghUZzLioRUaQTHqV7DBhmqS03ihGDR4KKdo4DJIEsu6BNiXBULcERrDM9yoCOaWdFoFhvymiWRVVYhzBWftcWQJ7X1pCrvYB7F6OBLYpGZ9EuBjcfxPFekD5vCXcF5SifRG31Aoh3rwz%2FeMAR2HT1rI%2FXNyzSBnXh1sOn0iSBcMvNH%2BciY7zczoaTxfZq8UXIr7seoy%2B5PlrQ2VgcyDV4XlaYVtpnOw9q890OzeQvoVUCUNKlfkMYMJ2A1nmkCQEo9xNQr4%2BfUiS11sUjuLaSCUI0rMKiuNNqbg2Di5XsRG%2Bf7nUuazt48HSyIzXHIDCJoysJCFy1tut4sxBTFo8wWvcs%2BDJtiiVsL96ExL%2FontwuK2eGNI873pBwkZF9Iw0thtUFfs%2Fo69q0iZvrVRmEd6jbXGSV%2FkQ7cPIkRz2HHNwAjzgdrWpUTu06zAiJXyT4CKttjrdQ2h%2Fp1%2FcVMSWLgru6UyJ4vpm9PQMLmBOeABIs7X9ZlTqceDSGaWq6ZV7slb6BjPrz7c9eQUrFWzXrIw1epLJAMMGAqcgGOqUBWSl0o9l9NSnOVAZTK2b9eI0QAndKrBKmr6YJwEDzmZMJYoxTYx2Nme%2BboMr2rcHnTFrm7ggR3k%2Fh4BvnyfZb755X5OGLcK46BKpA9D8acNtK1kl0psoSxrNR8tRRgkf8HzGD5%2B8lQ0IzXfUYAtf4KZJRD7bxE3MzN6vMmwuMsZuxnFtKXAhxUEajqkB7LVdmBBlaa1RXtLgTbHJfkd1DOs9bcLbU&X-Amz-Signature=5eab2b88fecd188d1de186d8a0a51a62343ce4a885782f86715a00e042281b13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

