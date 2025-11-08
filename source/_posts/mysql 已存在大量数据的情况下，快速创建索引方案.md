---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7COQK6%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIFAfx1qVqOzD4vqznSgQCvcCE5Gxu9XDcBgjx1dnNUrhAiABOy2Iaj2xWhLF85pA5hZYGwKtgfxIIrrQKR7ZtvOO5SqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGQAtBONpdKD6ChJSKtwDNB%2B3ox%2BSaejV%2FSmefzZmTXX86L1bYVNIBHX8EjMpHPnmCPXglOUKZqcd9UHANPOhydpGmNE185WKY53yIcEDOLUdJlpvnic2TYxyJ7v8GmAUWf6iUiBN5N2dW%2FyCn5nI6XKeL13%2BAHi91ivx%2FMFRXUltIN0VcTV0V0%2BMmAMcJdnnDNg7tKc1TDlJab0w0m91Q07fIkgzRRyQAVqpbYe562PDoLOiCCW%2Ba8dRg4Yge4zn0j7w8M9VGNrAwojdRII9WkUu7sm%2BtFChn0Yng1cD0eoF%2BmEU%2BoDX4gfrAvWShaARjhytfAONST4B97B98XkLBMISPclCLil74TYzkv%2F3wsdTO8vSoxots8zvdQ4ZlsLowbH5NZ3uEnYNzTEqOn9767pee1aAdPvUEjmQFM46K9usg5wjZ5%2B%2Bn0Km9u9j%2FCYt16iIqdxl3eo%2Betd4fZ96Q1MF2c2wppsjKRiwegTiE9oNHfvtXDOyQUuwJa%2BhjJuQo9foIB1No9CN3FW0sNYTwre2URcYf7vEcj3B52pXd0LbQLCynREWsb%2Fls9IA1dU0fWjxGXfHk3wCt8cyKpwdy1bNzkP%2BfaOligdT3wQETJ6Ic4jiizAI3SQ5w6DCAW%2Ff2PyjxhZ5G%2Bq4FqUwre67yAY6pgHQaZ7pIadooyVPBskgaY%2FXGlTmvo9TjlFHhEPYwGp%2BRJZlDDG%2BBVqK24WvkVHjsdGrtpJwM2imT9Gn6b8hESgF9IX0pvlxLVr0fMVi75JWfZJ2ML7jlMGkQzRQvRHb4HFXUpyPCSUjCc0TvnSBsOs8PrpwCCgqD40ujCRkmRSUXZPhy4NZrCln32TAhH8%2B3bMqSWrEbBzsvhnzpTf%2FR0f8zEsPrcKZ&X-Amz-Signature=9bce37cee89aa3eb1d67ac0a9099b48f6cfa915dce10f66a3901b7e5c9c0213d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

