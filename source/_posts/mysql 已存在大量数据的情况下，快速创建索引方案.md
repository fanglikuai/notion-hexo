---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665RRA7IMS%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T080049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEeiP3gumFtjXvlMyjO%2F1eTk4aociK875HLXKeui6oUAiEA7y1kiazSD2L25qmpmHlc3S3JX4qD3toZwAzzLKVT0fIq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDJ8P5u58mcYrLCl6NCrcA4zPTLoyJOzeehlNgq%2FS6CPy1sqZQfGmtvTw2Femf%2FzjwSqgdZgJIUNuOfOLl4lYGoEOr0vpe%2FPneKAziP4EBbvSeOMABtMba1iGEzkPPoHq%2BjpUIxcaa5LFOuGgeJv79INbb8QeX9GxItvKf20%2FqoqrcLMPwdkTZYqbMIsy14zeKUfV8kDh3WOnUM1n9opDCY0r3h7qOhe75M8NwCl%2Bgj8JrWS%2Ftp1PM18dKqW9zgeX27lY0ILyH4qsMERkikEN%2FhK97oSV2YlZ59dgeTIvShQMdXZ8FbRuR3brdhfvn5zYkqidHyuMb0j1b%2FEBeJnEWwDyaPoMyt9qWh5bTj7R3m3onoBua%2BLGBUDkW7MSHdczc1aKRWixHEHOOmfE9ouUrZKbzNYDdC%2FzMVNVuoryFBQRBhYj0Lb1HzINkrlWA3LPVVVgw13VySGJc%2BCr8JYNg0auFw7GCyvVUSRoDx2we%2BK6zEVuDv1Nk1zJWZWfPYMMdi61kitvA6Vy2iO9fBLuOlJYs3DcDIpNGVc8p6O%2FbcQj569rs7qPSKTvbltZOrpj3oTE6zyfYaaTGXe%2F8Bfaacjs%2F4ch%2B%2BAd16Oe86RazVYg4vSa0zGunBOtexHtC0zbakaUZiFIFnjdlxINMLeHiMcGOqUB1DsdJ9vTQIiZ60d8wkJg6fT7T38EzeeHKkGyanT%2FKLlXlJFbLlUhMojhbPD8xHSX%2BMSPZ1BPteTWnVtPFtgkD7f3rkbT3hH1kyEbnsVCl0pB%2FK%2F0DS2Hs2eSjufMUGiCb5zOyh4qqsBymjqi4tOjIIT6QLhViBt4nqvb0rLdCJlMbuf8vBnvSYnQaJptcOrz7%2FwJ5NkQB9LrescKA6Q5z69hOEiy&X-Amz-Signature=0648e1b05b4d4deff1407beb2a3dd826b203a82cfa280ccf3f8fac0c35495e57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

