---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXKS2KYI%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBetCnxvmxzMm36C69fjX9557MBuSUCBcZZUCLrb4OiuAiAkbioLa%2BqoiX%2F0dKHeLTzscJ35fvGZmUzQsDRDVtjlPSr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMl7JBZsmVfBfrne8vKtwDNz3ls9Ouqr60%2B%2BMc8scTHPgLlaYqMDG0d%2B3AJkL9ZUOi0blUsumgNjL%2B%2B4mtuF1FEFuGilA03zThiyoufvjUiOJQvZSFhvdk%2FteQo6x6ip01pMEGMR%2BhMv9vsAWxYh5a8c1%2FkwliPnIDEHMuujRvG2QPFAXTNe69NtjwLsDGLf9AUmUVRqed2W4migWfxsEMwFVKLgnE8l7cXCG3sJV1QYssmw%2FEFuwY0EAsbzBpDU6pkwModyR9tf9dLlcCdQj%2FN30ur%2BIGbGXCIEwZsCjc4w24St1YA960HFJOz2zMW9dIxNf%2BR7g5S8NCzudSTopDKrnqKXWj98XMw%2BgKYeCaEKmsAj90oz%2FlpRcTlVaDNDKMXtMBfaLEq8eagvd%2BsaS%2FQx1a1iAfP24lCRnk9w%2F51aE3a9NiZzzWznUdA6IVXaN3ztEUEpddcmDnWrwltKRfqJ%2Bt1JdlIWsfOQfbn9UN6TOaaCpqhPd94UY6yErhEQPlWnMSWFzTn69KZU4C8%2FPz4Aj6GO2zaVMXNzE7JWTiw0whSVmlOFR4NBfyl4qGSGoyRbXhN%2Bb8smmNAnZPb9SISz0JHfKrPjiTTEZODJo9dgMQ48UPy1scPtG1TrsLv7i%2F8%2Fu8I3EuOQv9qfcw772eyAY6pgHj%2BN3h2Xd1pYL2AkSV0mY58MIy8zWMF0M0F1jp3jpFSYq9mbfO9ycAXlc6H%2FWE1QRHK4NpRb%2BE%2FjGz1bihc%2FvRCZ8OifFaxaV61FV2IVvBd%2FXfY%2FYBSUelnARcDEVPalQnl3Um1UoZuz0rPaD6we7i7kWKCg9zH7GQG9B6LrGlFU56y7Rt52GX%2BxZ8SK0UMC8%2B0bY3DsySNwZE0eQTLAcsHQsnyQQf&X-Amz-Signature=02140c3d6171e9c4e2538aa10203778c2512394c1d9e53c857b8ca88fef3140d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

