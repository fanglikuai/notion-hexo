---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6EKOZ2C%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQDN%2BpIGH5Pk7UvCT9jYKqofJNmACizquMDHGWfF7yWoSwIhAIRyWah8Grff2z1Ee1IBgipZZ6Y1NleQ%2F9qKMsHUST2TKogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyli5dEnVRZIcgdO1Qq3ANAuMvIlTEiubYs1YQ0mxmQRRab98mXC1IIx31mTUTjxIRO9DSMSUYkOC%2FKoXq%2FCiL7UcvLOfjohyAiYtHqZkncpFXwCQuf8QnSTz3VcU9eBzsD5ARvoegh8cuKR%2FClhxspcjJLaTpFbNdGKE8zmzqzgW0lFSnH7xE7%2FF2b2smw8Yjn0ZgIu8WolifCAYVyxem%2BsX6uMQISWZRMhqVHgmspEOzCBEWGMDwlTtBUys0braIlIyGiV%2BiGv8zduPbbfx1csAP5bKG9Cn3Nl9i%2BdW9HJrIVo3FtJINxtTocihqZH1XmxDeNYBaFQevPvBhaLWxLylqSqZMrdGsgMlnsqd9IWVKov0GbBU5raXsiLDAOohjwwnc81k0HBZqmqMiLKTpudtRnIkAsK0ZGRLzLlhAuxDf5Rp8Ac%2Fh1%2FZ5yUYtEakD17abcKodfo5nwrxXoiDGVWe9Pjw83c2PJAon3cu5PcEUa2AfuluVnc8chpKmilr3y7%2F9x%2FkCSZCrddxaDxS0yYai6jwITsv0wIMdBgONjiTfeIpMv2fjl%2FapnTGLAx1BAhEr%2BlwWAG52UD53weAZQWGEcnHbpIBXx6nhQAiHFTd%2FTmh%2BlEL4rzmebtML%2BziJ8Yfo8KW%2FMX%2FezrzDNv4bIBjqkAZ0Mtu%2Fu6a0awDisx5EfUFrs7j%2B2k68QnPJ72df6VwBIfcrPbkHswoIoM%2F2c1y%2Bd2yKrmLk%2FeYJC49hbevUW5MJq8GKl6HFDCchIPhVZZqdwFnEt8DmO%2BlyRp5gCyWhoQTq06P1Qbs5KKHQcRmPdtaBNyCRqdmohM1rigTf3wEOpeDZxG93VCh607V4oBYdro9hYV3GZVnWb63gMPu8PRlsrk1JQ&X-Amz-Signature=2f41a5e9c5af2f0bfd02c9cdecdeaf93f7f868ffd7849657e3817ce2dddc1f20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

