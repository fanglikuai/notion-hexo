---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQ74EODU%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGarWnwl1D8mdclh77jFfWHm2%2FNmdGirUzAworzc5KCUAiEAu47XdEyQ5yP4QTHUsiri8IErgXwa8%2BojrnP0oc8YjpYqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJkbRHKbSUz3UDFaiyrcA0Pf6Ou12Wt4SnwFkInr4hEatsRsKZV4w3yvU07TiffQebKeB%2Fdpch3hNMutT6EBm%2Bi03DLCsTd71youRMpcYPuCTenn3h%2BNRljig0SCLFbmI8DhO824SdNhDv9ZKJizOTwNhibv1s4Z2SqjJ8K1cKnu7HUVvaUqJzVp6x7oCdK1YX5RZL9QGlayAs54T4kmspZIE2SDGZwaFdiaOdZbi%2FDkLq87GN2cdID2%2F%2FCBUOXy8tM%2FMphh3xap2U7lKSm6a4AI%2FPXsFENLEGbXaZDW9k3iUqOEa65O9ZyO8Cka%2B2Zd6N1euD3xgj7TN1VJJChobK1jHXx%2BzQBKsdPe1pGPfN0jr4QLmd3WpL6RYeuqK1YS4rrqUylGQLRJ9cdmTWe3sonYgOHkXTeVWnkYGtw0eK7zq2RHrCXSW7wBAN%2Bxom%2FG4ePmJnTLyikRjRH9TQlVyQ8757dFAnB4DzUU9XPq60%2F%2FXhXHSX57%2FAT9MPIlG3pgz22d4r1qw6R6Tx0P0QPVl%2F9fwiCRpMMGYYZHDftcGmjzi7KIDvTYEG1YODHW2kk3ksQErFEpZ9Elew11rccSXQfKUvfctARCLaxb549NdvqNIRc0spUP8qUdVqnR9xQHCu7fFYXgR6vVvM09MIeiockGOqUBkZBUu4NGnL06altAfUWTTSbjflQu75GAv28Jhp%2Fxh6bIf%2BsFY%2F5tWYD5i%2BdxY7UYGGJw0DleQ%2Badaow%2BfB2YiXaS49gND%2FzrBJvd4ZtccD6%2FsDFkZH%2FueqsiMN%2F1naSH5HB7v3pF8bU4Dm%2ByTOIxXX8%2B9nih0dY5Qr3F%2FOrkhHZz9us20fD%2FfyxMJgh%2FXbWZ47G1TLZ6mhZnFIZt5uw%2BDhsSgZct&X-Amz-Signature=c201a7c45711bb8bcabd1e4418d5f0bef721ee1c9bc9282ccdce4a76b4f1180f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

