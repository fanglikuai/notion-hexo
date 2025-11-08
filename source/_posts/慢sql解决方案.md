---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZSAI7W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQCzpXhKLqDIVtgOCiB43tZ5X6RgNVUFURth%2Bz3ZGQQ5ZwIhAO%2FsAxzyac6Qov2IdVj0HEVAwzmOHyiGFxKd%2B0XWA%2FpbKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzRXZWGbMSF4yOg2Vwq3AMNpfOagOSQ6dmWfn1oEuhDNvZm9w8mjovdBz96fY55gd7GH64oXyMJuLsznjq0X6W5AXlPRQSUrSPbja3zU8pnP%2F0T%2B7ewZSZMU1mE%2BGqoioc1Yryo1cz3dyl9rPZCDpirkrZg9ZcCdRMx2zLU4OLOORY8NBvDNpo887SQ5KLkpWKMf27U%2FDWlZvJkCFlD8GIpg6UTERLzjx04wcCtj7kbXfwIs3am0Z48M5nyJ3ZkY9uWYzNKSGRlH3McH9k7ROUyckHudThyVKLgZiyu8vr1Z0RqtLkPLVyroglAdL9k%2FPWOFVV1%2BjEAw0xDTFb3fRl7QAwUgIvEl%2Brth4YqyfCSCgZfgyxwWmF4S9Itdj9cYb4GbC8f31W6CRnTQuRl%2B3M6iv550D%2FTF%2FTGY8peH63cxvEPQA5FztOcO1Kyk8uZCXMb%2B7cdjkbAEyPQeuSvy0N7g5Dz9xkDm%2F0NZL6H5K0200mjqpfEgEh06A%2Bf6YwkxbjMN62AWPsaEulRBxEBAsnd807g9F%2FsKFl7lZ0ccdYJ0YJzr8Zm%2FtT1mUHRd%2BNR1v2qlGxoR5IeRYtGarVSQagzAFUw8%2FyDa7ZWOq6rkZELQIYUv2A%2B%2FVk2zzEgXuy9v8PbPLbyLEx8WJjOSTDq477IBjqkATqq2eteBGMSGenmQUj%2Fh8yblU3N5CpVlePSIkQ0pTw1OHLcW%2FpHfsTlVv1ldFie9zCSPgmovCMCYYI7O8ES%2B8ncnONlgR02fJnA20l00A%2BF7TzwmdYlyeCBFcXT0SOSLqkH7X%2FBuUaKIJqjVVLGTt9Jm8RAD39L%2BKRHO80HB9fVcpCShv%2BLWfwDFltsQaUlAKd4qsZHBjLxQk2r%2Fgynx7c8I2qF&X-Amz-Signature=c616c2614542ccb4e0a347c8cee62e4a7a94e8882eab8693034b690daa9e1cf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

