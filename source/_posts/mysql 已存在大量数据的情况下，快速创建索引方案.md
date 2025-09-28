---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDJASEAF%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIFYobWmFcLSjv%2B5m42HGoOjG1yeDhN%2FXMwXAUtmM7q7dAiBI19snlBthXMiux5GE6KntZ84MlLikeyc8hGKJar9VhSqIBAi7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQCAOpQrkiN91PtAvKtwDzSvVA%2Bd77yCjU0xPCeN03FMLxTh44Grll1AaClnMu41sAUWWOXkGt7oFVyTMPFaR6JDVJr%2BURQZdJ5h5t4wKGwmJqtwr1f4KBX627RONg8WBdu3f%2FtlLRMKQtaSMVRKaXYqGu36ddiWOg%2BRj1wu110zqVIgMBn8XDjXO6M50an5ScBvFid7L8YBU3ehBZN5nEoWk82Q7OC39ZW8QfctuEe8q66Uewi2dC1MWRgjVTyZm7E9DCS9f6%2FA0g6j1wxXxSlr%2Ft78OlRBVWXeqW502GlmbuZWYIQGig45iIg4oS0dfy%2Bddd1IlyGQhjlbq7inh3YaEwiK9FmV7Vr6eT93A6MLg3TaHMSMrJV7cC3EXr1uoH6EY%2FkpwcCnLUnvOF3JYUaEBZx64lQlaoc%2BO%2BsX3Odzh0qSOY6gl2716iLwG4O8xhrgWDvlxGZCbGFAV8o%2BBzL0rztNqtWPJt4fmjCpC%2FNs3a9m4gV38OcQ3ad005vPADm2V2p0Vk4Q4A5Ev8IXLXkNRVztgqArL79VwNzqkldUFm6C4GqsgsjnrgTxfWcLoRsNegkcWEjI3wGls8Dp4AThDI5Alj9yG1eBXTODYzI11y8RcZzi3JddPsPxSrwX3roRAKHxV9b2UDCMww47kxgY6pgEjAi3kniMHYEHVc4%2BnLz28isSux9pWH8LwHJlVjnjPjlwwsXRNPBe32rbsSezPub75TyprtN3fBIrCGnO%2FyXY26ag2nYfHpQ4Ti9gM%2BfXhFFmMkVRLF2kXDxzsrlgieCFtW9cb7%2BOZGZvWIouFcPDWH7QNbYp34NN19Cl%2FlkfU4tDIn5Dzt1oUEHSQpT4gTghHWcVd5fuRo3%2FU2goTVh5lwDDLHLx1&X-Amz-Signature=6c4dc067dad8054a1d129a596acdae9f28822cd7d845617b3a82c0e0311f9e98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

