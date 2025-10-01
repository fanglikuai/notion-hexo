---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVOHTAFW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDb%2B2tzNnSr%2Fsd77aj7LLnszvbx0WeKxDDq1F7WPcR5swIgVmHpPS%2BpiVLRR2twR9lJivONIZAgO1YqvilNPOIA%2F0wq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDEB0AjdlqKlWspAhVyrcA0wXu56wSkfuvDlaJpnL4TpEnXbbGxAH7oCY%2BVfxI76XPvnWrmHxWvo1PEbsYRFhRqHXHXEdee%2B80%2FZ6ZuqYHpSOWp89ggi351D7jGnDMZ3Ji%2FfQOH9cjDOtgpcGfP4XUBrL3PBXX9aWNnACrC6eRE8EVseYDC%2B04iUpJaZ6TKiLbmM1i3s%2B0iHXEDRnQDgUbFt6N9FQMdqwJGnK5Tn0F2XRH%2FC6d9YWDFpuCjwbwQz8vSrV%2B2wt%2BiMmjeUMc6PSS2XP9F5U8Cs1jVCYjnErd75y68RFg99NQjqPhpD9ssTQF3UDIz1Ccs7eUZm4YDTwvVr3DIOaV1HtpdhfbineEw0MymXmHA2yW7t799NoUgdUM1%2FbL03SeF5yCoRGqsMVPjdfZ9UXMgnsXtMVYLTe3t4mZe5y1hCRFJVVRwzLfQADT6ADIZC81PPTNXVS6KK55PdVngm%2BoKivovz97dycZHKQDRHzy0riV3PJAPj2G4qfrmvCmKx4S1bwCJXthUWU2%2BA5eG%2Bk5COszsfBPsPoBDi9rauCBObxstJcUQE4UpRVwWRT%2BhnC25j5kBRUMbO4fw%2BEZXfGjCIcseKUnHh1m1Ksr8gO4%2FsSUvIeKhTxPURrouyXqIUKgKrsB0l%2BMIny9cYGOqUBeWZxQWuOZZKwiws4JGPoJCdxn%2FtmOty%2BeW1GjOrVmA22kVrXRSEK6Movru0PwNgN6s5BSch3uuvfR0wcJmkF3zNj8Ukflzcw4ppHaXH9r7axyIP1ymOdUsbGuuyK9Y7ckMfBoru1r6%2BMPvyff9nEAfwfZssPIbHTWj8mV%2FQhrjbd4YpnHpC%2BVEEjyQmUQyVS%2BkTuOdWS0EK8bKDAp37y8xfd12yr&X-Amz-Signature=6d6b86e67f435b419d85f9e7106c9e65729ad51c1f3d7b32286ad892bd066275&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

