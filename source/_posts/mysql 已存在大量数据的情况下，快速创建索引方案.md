---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVOHTAFW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDb%2B2tzNnSr%2Fsd77aj7LLnszvbx0WeKxDDq1F7WPcR5swIgVmHpPS%2BpiVLRR2twR9lJivONIZAgO1YqvilNPOIA%2F0wq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDEB0AjdlqKlWspAhVyrcA0wXu56wSkfuvDlaJpnL4TpEnXbbGxAH7oCY%2BVfxI76XPvnWrmHxWvo1PEbsYRFhRqHXHXEdee%2B80%2FZ6ZuqYHpSOWp89ggi351D7jGnDMZ3Ji%2FfQOH9cjDOtgpcGfP4XUBrL3PBXX9aWNnACrC6eRE8EVseYDC%2B04iUpJaZ6TKiLbmM1i3s%2B0iHXEDRnQDgUbFt6N9FQMdqwJGnK5Tn0F2XRH%2FC6d9YWDFpuCjwbwQz8vSrV%2B2wt%2BiMmjeUMc6PSS2XP9F5U8Cs1jVCYjnErd75y68RFg99NQjqPhpD9ssTQF3UDIz1Ccs7eUZm4YDTwvVr3DIOaV1HtpdhfbineEw0MymXmHA2yW7t799NoUgdUM1%2FbL03SeF5yCoRGqsMVPjdfZ9UXMgnsXtMVYLTe3t4mZe5y1hCRFJVVRwzLfQADT6ADIZC81PPTNXVS6KK55PdVngm%2BoKivovz97dycZHKQDRHzy0riV3PJAPj2G4qfrmvCmKx4S1bwCJXthUWU2%2BA5eG%2Bk5COszsfBPsPoBDi9rauCBObxstJcUQE4UpRVwWRT%2BhnC25j5kBRUMbO4fw%2BEZXfGjCIcseKUnHh1m1Ksr8gO4%2FsSUvIeKhTxPURrouyXqIUKgKrsB0l%2BMIny9cYGOqUBeWZxQWuOZZKwiws4JGPoJCdxn%2FtmOty%2BeW1GjOrVmA22kVrXRSEK6Movru0PwNgN6s5BSch3uuvfR0wcJmkF3zNj8Ukflzcw4ppHaXH9r7axyIP1ymOdUsbGuuyK9Y7ckMfBoru1r6%2BMPvyff9nEAfwfZssPIbHTWj8mV%2FQhrjbd4YpnHpC%2BVEEjyQmUQyVS%2BkTuOdWS0EK8bKDAp37y8xfd12yr&X-Amz-Signature=322708c7c175689c632324855a0719f832a165340b92dc06cb073a0e60720c6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

