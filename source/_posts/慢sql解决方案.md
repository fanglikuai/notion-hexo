---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCBPONSP%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDM0rSw5KeFK3JmRp%2FP%2F1%2BXgDOZpv9vVRIbjGav7hG%2F0AiEA%2FTglmc89XERhiX6RiLc7FcHNp0t%2BR3oLdgd9Ga5go9gqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjavvI0yUaZ1rkv4yrcA60U0R3nbvd%2FMB0KSNK9AtDGkz8oFkS%2B7rbeLDvE1M6EHsQq3WCFSMzT5Hvw107Mosb0kOyzkkcNo6eIGj0vSmx2sNzxEaOfw37TXi4TAreYUrTdFkAfQL9j%2B1VOLe8PBiRZc0VwZMLthNJ93OSV4wK92SuUNJTA6MCM3RWxt%2B%2F2hb2pwa8nUbOp8MjQuYjTJn18nJk26vv9zEpjtnRQoHHjkYZCB%2BY30OkpSCBk7IGAQuYxI146cHrQHR5p%2B5qZ88SljjWe94pcgNxaRC5tZHiJ8LcNzpk3XtNSF36XITIxRK%2FS5HSouuC5UaXc5whfE2pL1xt1jeiezCZwwzca0R6zZ2zeH9m6vPkL1GDr7HNfm6uwFNiGuLJwHF0cI9y%2BmnTRnSx7%2FpGU%2FvjHWtStZk707YyDHsGCJUGv4zr90TtE3nE%2F%2F%2BK5XQK73SoNgGT6Rm%2FsT7O6T2iddHKzM4LaIx1eqXz4zz71UJ%2BN2%2Fh3VFK%2BNtNbwwvOEA5qKd3axzFn%2FWdnij1EZszJWsSkF9ecbha1RQ8NfJu%2BvmNLVMaHuxmcbISdlnuKOlHogKA%2FECCt23OAjspBLeMl%2FDUN9%2BiaEOUmCxgUD9DOrTDBz86oqETjIyDSNZLABteM4S8ZMOnU6sYGOqUBLdK8ozFRPSgdQRPryS4N9kvDjhxGdniB1ERGRBboaEDJIxBlvPficiR0tvB2gjJFuPm8AShGq%2FXhdBKMvbRD5%2BQOT7MJO5k4rYe9Vd6ryOHasfjuqAwJ4%2Bm%2BOW5iURIlpK3KYJmdxWaSZUhKXuHMR9lK6Nk4vEphGw%2BTAPDFiZo9ZEqrgauB%2F3Q%2FFJ48l3uNn1sI1HL8378o3ywzjEaDG7%2FamKQ3&X-Amz-Signature=6ba366dd72a5bf80d447654f650b621d11a38c11f255c83b955feee1ab8ad5c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

