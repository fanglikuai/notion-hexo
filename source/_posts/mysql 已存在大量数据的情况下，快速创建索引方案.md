---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VRJL2FY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDuw%2Fb4BrWq85t1jP2CX2L%2FKhdOcgM7Bf7BTJeLhLoQBQIhAO4ZXwB9YQuA%2FL9rxfCMgx88MhYPPGDlaNmd7WopNO%2BAKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxSPVGicUItyeWuPT8q3AN0Cowtw8HGxt7yRa%2BAX%2FFWzmSM%2FVWt1%2F9KkWjgq7r3LC5g2CyOxh2qvpevPyDtO8isbGtkm8mrGQEAFQwbooUGSe4nplHTZwD8IE9TokngIV4TrRVbe0Uy6R2i2t3RWez0hUCg3h0bAEUUv6UxqeT9f2KBadOL9RGwyJlEp6dt4IQOLtNdqRTzX8AY27fp%2BOp0vyYzCfGkuHULahXW1uFBd%2FR8KiT0o5o25Mo1tIMQ7QsmXyve5gdPx6GiYxIou1DwzxEUqkSPD7Iayz2OU%2Bt5ub1lX5fFcqMz8GMf23emtHVeZxgOIol3vV1eBfm79pbsd%2FA4%2FzAcKgLo88TCaLHRt10NAahIM1ZVWFXlt8DAw7j2vc8cXrko9BX0MPqavcw3oGixB3uir8Q3oXtdCpXG1rDk1R43DLY6Qlo%2BLotHQNF4OGBKKcEnSLR7C0zldQF0MCW%2BdZEmgs3%2B0I8iVSlhHpUlgleWHHO1r8ul0ZGZO6GstdpZLzGLw%2Ba1UQjg%2F7OxvkMkO4ubmXIMcv0%2BAEnyTxtqGRf%2Bw8AktVwtnbT0xo5oa99%2BsSdwsrT6j56eM1iFcLbRbdyahF0ZIepF9GHFNkmNehspBKhpOD6OHkcxbAfHD238fH9eP6snvjDJ7r%2FIBjqkARkydrzxazCcyVIL3duf5SjP5uxYT8kmP6%2F%2FEUTGXCK2%2BS0ZgFINLqx2%2FcKVMmYC%2FDMObupEOFqky4o6FitjKxInT254B7xOenpjEmDHsE7Db99DQJBhJVw16kTL7K%2FNm88AyWID0zC6yIcVAtw4E22vSjwFuXltFiwoSpHkuX6vkLGDbRZAptPqIPDKw40eBGVTXuI0S5KrIU4ic6l0dtHSZcpc&X-Amz-Signature=f9694e7425f718ebda72dee32a669d033afe42fe8ae12e7431fa7834a6eca118&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

