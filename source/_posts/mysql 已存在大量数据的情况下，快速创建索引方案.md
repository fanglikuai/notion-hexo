---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSHJ64KK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T120058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIQD%2F6RRxC4Sv2sMFkkcWhFw0527APCJsb6eDIqyeIu2YFQIgLEUBeGHeqVqTO5BbABiFtfJjRXnnJ%2F0%2BwPd8yvIww2Qq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDL%2B7rNz8wD83e%2FHyWSrcA3drm3IrYGW%2BrkEcHh1IBLjn3q%2FU1CCOVSEVaiQAiFusIqiNqJknDRem%2FH4XRPcz41JuI%2BqHsJwOa6HHT%2BG7XtZYau2udOlRec4PTYY%2BQndXwci6Hj7sl4HzC8ETpy1b3RArP5TK0cHHrkTHST5%2F9PVXVwSSWgl5crfCOsuSGmW1qf0ktmt052htMCJE1wDkBPVsQWIH0U4hJ9BFXRcPBnS2V5BieB6USPP1jM0vxsL6KAxp%2B1Mfdqw1GRkt8Rdt4yxzI11n7KKgYC7pTm9izo3JRYtroRkUbzbnbX5buD6Yu42VtKWVeYn1o7FlVht2SwRVmOlPsZoNmNwBJNBHeE%2F%2BPRR1MFRXmGCQTc6TwdwDORa0w181YMQkklxizgLej%2BuTaIhp9aq5yQqudiJ69GFfhF8fP9%2FG9f2ZBTAI%2BEmXLDqcgan8ZRA2%2BtWeAP7qE6DNwPwZ1bHUywlI0vI3hovk5Di639FGTGFtXG9Bgu29xHSJAan%2BTZGSjvfbqSbkY220T%2FNkoaR0%2BiwsZzsQwAfJlOG0CrYi5AAuld0fdU0Um74Pti0igBO73vzz8MJbfoIT8u6IOSyDxRxpXFSNlhvjotSOG0WwZFgJEFzL19Uhwyli1C1FViRkDBtrMIrQ0cgGOqUBZDW0OkQMIImILVwroL3uSw2xSqxuR%2FDyxj5SdsIm%2BUPtbVlUGT4B7T4kxPKIQx8vaZ7JLPKJyYAFSk5twYig2fwBgfmdDeNf9O%2ByeWv0mBm5rDiBiPd9omrnhSgkiWsF8cP1hjC6zMuLQPAMgCIi5ged3mIei3OVq5o6XM%2BVYzjia7a8syQ02GGJZHkRiOEbAP7s11BeecacUX3FgKbsND17H0J7&X-Amz-Signature=98180f8e646bdd3f3e82c349ab113c30d7e2378c9eb94d5684f6338b9762b5ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

