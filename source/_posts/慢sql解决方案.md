---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTBO3U5B%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWyJh7GcywTWMocmxSuH%2BXv3GJQHhDVDSrtY2S%2BABfWAiBOT%2F0iwBCiCgadlcUhwtbKq63ruNT96aiiPDC7FnCmgir%2FAwgvEAAaDDYzNzQyMzE4MzgwNSIM53%2FKh9O4ZHR%2BD363KtwDTASjY%2Bj5Gqz1K02kknj%2FP3I3h%2BYYqLWle0Dq2Yu6f4TDx1NKBySF4RRe7H7X8%2FNX%2BAg99F11XutzQbZ1uhr76AKnn7b1bI6D1nmSs4IO18nZHAmXojc4aDVylP36RQC8UsCHHhuVGXxqxPtbZk1FNuasZ3JjiquNkdVVjlLA4cwtbfhh9prkxkoBmm55YnXLCMCvmDJGAbJsYEZVHBSKGRgueOSvBp1IvNrnk0BVEUFobwJYbxHxGSpsMqwgApkriHxr3TRDd3qhBsTooL%2FIYlM3XEHjzG8hbSpGuD8VrC1cN%2FEXStg07hQh6fWgOpNJX3SRocaKd2C2zghhsK23uw8vsOPrZWbTOrdD0fKTvKBOokHQ%2BG6OjBBxGY1%2B1sbiHGyfgKCc8oVdoph3VRLKfz%2BIhhgRY50rf3Puj%2FFUQxnXYoDVU84W%2BxBAsGGTyqNlgnS59mABv%2FWyVXxmKm0%2BQyPmho3YQ9XblbdZwtcAOzC6H4JeKEpRHWB50DwIiiwsDUCv3DmIOgQMXFg%2BjtxFLKm0NfawRFmzeNiZTpj4UAvtH4jJWexDCdDarKB8BTQVB8TQBQJdFNYuqTSfqjc2fQ4BvxW8c5f6EPZ%2BDORKDDRprlReHVeKqgGvxNYw2ov6xgY6pgFAuitGRye%2B1RjNkUJP%2Fabu2Iop%2F2NgeOSy2MbhdS%2FTNWfG3z2ZuA%2FyZ4SWcGflxx%2FMC112fcqmfTj8omROtaOyuPP0nO2T48%2BoTfgOgxL%2F3eLHBceaWGyBXUPfli1VqFHI9WvUWMO0uqVQJ3laGeqjT4porwncwoLlC%2Ff%2BKJqpDmCGcDP56PwRsnGxId5Kop90kKauhz%2BG32cEZGQcmiaXpM4NUAg1&X-Amz-Signature=f94f27e493b33ce19ffe960f2cbdc27e0f073ea1f9e43df3c132d23453b303b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

